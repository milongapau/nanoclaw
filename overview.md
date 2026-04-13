# NanoClaw Architecture Overview

**Core concept:** A single Node.js process that routes messages from chat channels into isolated Linux containers running the Claude Agent SDK. Each group gets its own container, file system, and Claude session.

```
Chat Channel → SQLite DB → Message Loop → Container (Claude Agent SDK) → Response
```

---

## Key Components

### Orchestration (`src/index.ts`)
The main application loop. Polls the DB every 2 seconds for new messages, applies trigger detection, and dispatches work to the `GroupQueue`. Manages two timestamp cursors:
- `lastTimestamp` — global cursor of messages _seen_
- `lastAgentTimestamp[group]` — per-group cursor of messages _processed_ (enables context accumulation between trigger fires)

### Channels (`src/channels/`)
Self-registering messaging backends. At startup, channel modules call `registerChannel()` with a factory function. If credentials exist in `.env`, the channel activates. Currently supported: Slack (core), WhatsApp/Telegram/Discord/Gmail (skills).

Each channel implements:
- `connect()` — authenticate and start listening
- `sendMessage()` — deliver outbound messages
- `ownsJid()` — claim ownership of a chat JID
- `setTyping()` — typing indicator

### Database (`src/db.ts`)
SQLite via `better-sqlite3`. Stores: messages, chats, registered groups, scheduled tasks, task logs, Claude session IDs, and router state. Schema migrates automatically on startup.

### Container Runner (`src/container-runner.ts`)
The most complex component. Constructs Docker invocation with:
- **Mount strategy:** main group gets project root (read-only) + its folder (writable); other groups get only their own folder
- **Credential isolation:** containers receive a placeholder API key; the credential proxy replaces it with the real key at the network level
- **Streaming:** parses `OUTPUT_START`/`OUTPUT_END` markers from container stdout for real-time delivery
- **Timeout logic:** idle timeout (30 min default) resets on output; timeout-after-output = success, timeout-before-output = error

### Credential Proxy (`src/credential-proxy.ts`)
HTTP proxy bound to the Docker bridge. Containers point `ANTHROPIC_BASE_URL` at it. The proxy injects the real API key or OAuth token on every request. Containers never see real credentials.

### IPC (`src/ipc.ts`)
Filesystem-based container→host communication. Containers write JSON files to `/workspace/ipc/{group}/`. The host polls every 1 second and processes operations:
- `schedule_task` / `pause_task` / `cancel_task` / `update_task` — task management
- `register_group` — add a new group (main group only)
- `refresh_groups` — sync group metadata from channels

Non-main groups can only operate on their own namespace — enforced in the IPC handler.

### Task Scheduler (`src/task-scheduler.ts`)
Polls DB every 60 seconds for due tasks (cron/interval/once). Runs them in containers with two context modes:
- `isolated` — fresh container, no session history
- `group` — reuses the group's existing Claude session

Scheduled containers close after 10 seconds idle (not the 30-minute default for interactive groups).

### Group Queue (`src/group-queue.ts`)
Per-group FIFO queue with a global concurrency cap (`MAX_CONCURRENT_CONTAINERS = 5`). Handles:
- Message processing serialization per group
- Task deduplication (prevents double-execution)
- Container reuse: idle containers can receive new messages via stdin instead of spawning fresh

### Security Layer
| Component | What it protects |
|---|---|
| `mount-security.ts` | Validates extra mounts against `~/.config/nanoclaw/mount-allowlist.json` (outside project root, tamper-proof) |
| `sender-allowlist.ts` | Per-chat message filtering (trigger mode vs drop mode) |
| `group-folder.ts` | Path validation — no `..`, no slashes, prevents path escape |
| `credential-proxy.ts` | Real API keys never enter containers |
| IPC auth in `ipc.ts` | Non-main groups cannot register themselves as main or operate on other groups |

---

## Data Flow

```
1. Message arrives on channel → stored in DB
2. Message loop detects it → checks trigger pattern + sender allowlist
3. GroupQueue serializes execution → runContainerAgent()
4. Container spawned with mounts + placeholder credentials
5. Agent SDK runs inside container, writes IPC files if needed
6. Host parses OUTPUT markers → streams response back to channel
7. Container idles → closes after 30min (or 10s for tasks)
8. Session ID saved → next invocation resumes Claude context
```

---

## Storage Layout

```
store/messages.db       # SQLite (all persistent state)
groups/
  global/CLAUDE.md     # Shared agent memory
  main/                # Main control group
  {name}/CLAUDE.md     # Per-group isolated memory
data/
  ipc/{group}/         # Filesystem IPC directories
  sessions/{group}/    # Per-group Claude session data
```

---

## Skill System

Four types: **feature skills** (Git branches merged in, e.g. `/add-telegram`), **utility skills** (ship code files), **operational skills** (instruction-only, e.g. `/debug`), and **container skills** (loaded inside agent containers at runtime in `container/skills/`).
