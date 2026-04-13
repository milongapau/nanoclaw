---
name: financial-analysis
description: Financial analysis and modeling — DCF valuation, comparable company analysis (comps), LBO models, 3-statement models, competitive landscape decks, spreadsheet auditing, and data cleaning. Use when the user asks to value a company, build a financial model, run comps, do an LBO, build a competitive analysis, audit or clean a spreadsheet, or check a deck.
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch
---

# Financial Analysis

This skill covers investment banking and equity research workflows. Use the appropriate section based on what the user is asking for.

---

## DCF Model

**Triggers:** intrinsic value, DCF, discounted cash flow, WACC, terminal value, equity valuation

Build a DCF model step by step — confirm inputs with the user at each stage before proceeding.

**Steps:**
1. Identify the company and retrieve financials (revenue, EBIT/EBITDA margins, D&A, capex, NWC changes, net debt, shares outstanding) from SEC filings or public sources via WebSearch/WebFetch
2. Show the user the raw inputs block and confirm before projecting
3. Build revenue projections (5–10 years) with explicit growth rate assumptions
4. Show projected top line and growth rates — confirm before building margins
5. Build FCF schedule: EBIT → NOPAT → EBIT(1-t) + D&A - Capex - ΔNWC
6. Show FCF schedule — confirm logic before computing WACC
7. Compute WACC: risk-free rate + beta × ERP + debt cost × (1-t) × weight
8. Show WACC build — confirm before discounting
9. Discount FCFs and compute terminal value (Gordon Growth or exit multiple)
10. Equity bridge: EV → subtract net debt → equity value → per-share price
11. Build 5×5 sensitivity table (WACC × terminal growth rate)

**Output:** Excel (.xlsx) with DCF sheet + sensitivity table. Use Python/openpyxl for standalone files. All derived cells must use formulas, never hardcoded values.

---

## Comparable Company Analysis (Comps)

**Triggers:** comps, trading comps, peer analysis, valuation multiples, EV/EBITDA, P/E, benchmark

1. Ask the user for the subject company and any preferred peers; if none given, identify 6–10 public comps via WebSearch
2. Retrieve for each company: market cap, net debt, revenue, EBITDA, EBIT, net income (LTM + 1–2 forward years)
3. Calculate: EV = market cap + net debt; EV/Revenue, EV/EBITDA, EV/EBIT, P/E for each
4. Compute mean, median, 25th/75th percentile for each multiple
5. Apply median multiples to subject company to derive implied value range
6. Output a clean Excel table with color-coded headers and a summary implied value section

---

## LBO Model

**Triggers:** LBO, leveraged buyout, private equity, PE acquisition, returns analysis, IRR, MOIC

1. Confirm: entry price / EV, entry EBITDA and multiple, debt structure (senior, mezzanine, revolver), holding period (typically 5 years), exit multiple assumption
2. Build sources & uses table
3. Project income statement and EBITDA through hold period
4. Model debt repayment schedule (cash sweep)
5. Calculate exit equity value at assumed exit multiple
6. Compute IRR and MOIC to sponsor; build sensitivity table (entry multiple × exit multiple)

**Output:** Excel with Sources & Uses, P&L projection, debt schedule, returns summary, and sensitivity table.

---

## 3-Statement Model

**Triggers:** 3-statement, three-statement, income statement, balance sheet, cash flow statement, integrated model, fill in template, populate model

1. If the user provides a template file, read and map its structure before filling anything in
2. Populate Income Statement: revenue → gross profit → EBITDA → EBIT → EBT → net income
3. Link Cash Flow Statement: net income + D&A + working capital changes + capex
4. Link Balance Sheet: retained earnings from net income; cash from CFS ending balance; debt from schedule
5. Verify BS balances: Assets = Liabilities + Equity at every period
6. All projections must use Excel formulas referencing assumption inputs — never hardcoded values

---

## Competitive Analysis

**Triggers:** competitive landscape, competitor analysis, market map, peer comparison, market positioning, who are the competitors

Two-phase process — get approval on outline before building.

**Phase 1 — Scope:**
- Ask: industry/sector, subject company, geographic scope, depth needed (quick map vs. full deck)
- Propose outline: executive summary → market overview → competitor profiles → positioning matrix → strategic implications
- Wait for user approval before proceeding

**Phase 2 — Build:**
1. Identify top 5–8 competitors via WebSearch; for each gather: business model, revenue scale, key products, strengths/weaknesses, recent news
2. Build positioning matrix on 2 key axes (e.g., price vs. capability)
3. Synthesize strategic implications for the subject company
4. Output as structured markdown or PowerPoint-ready slide notes

---

## Spreadsheet Audit

**Triggers:** audit spreadsheet, check formulas, find errors, QA this model, sanity check, model won't balance, something's off

**Scope options:** selected range → single sheet → full model

1. Read all formula cells; flag: hardcoded values where formulas expected, #REF / #VALUE / #DIV/0 errors, circular references, inconsistent formulas across a row/column
2. For financial models: verify BS balances (Assets = L + E), cash tie-out (opening + CFS = closing), net income flows to retained earnings
3. Report findings as a prioritized list: critical errors → formula inconsistencies → hardcodes → warnings

---

## Data Cleaning

**Triggers:** clean data, clean up sheet, normalize, fix formatting, dedupe, standardize column, messy data

1. Scan columns: detect leading/trailing whitespace, inconsistent casing, numbers stored as text, mixed date formats, duplicates
2. Report what was found before making changes — confirm with user
3. Apply fixes: trim whitespace, normalize casing, convert text-numbers, standardize dates (ISO 8601), remove duplicate rows
4. Report a summary of changes made

---

## Deck QC (IB Check)

**Triggers:** check deck, QC deck, proof presentation, is this client-ready, check my numbers, reconcile figures across slides

1. Read every slide/page
2. Check across four dimensions:
   - **Number consistency** — same figures cited in multiple places must match
   - **Data-narrative alignment** — claims supported by the data shown
   - **Language** — passive voice, jargon, vague qualifiers, grammar
   - **Visual/formatting** — font sizes, alignment, chart labels, slide numbering
3. Report findings by slide with severity (critical / minor / suggestion)
