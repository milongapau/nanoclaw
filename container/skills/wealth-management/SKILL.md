---
name: wealth-management
description: Wealth management workflows for financial advisors — client review meeting prep, client performance reports, financial planning (retirement, education, estate), investment proposals for prospects, portfolio rebalancing with tax-aware trade recommendations, and tax-loss harvesting. Use when the user asks to prep for a client meeting, generate a client report, build a financial plan, create an investment proposal, rebalance a portfolio, or harvest tax losses.
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch
---

# Wealth Management

This skill covers advisor workflows across the client lifecycle. Use the appropriate section based on what the user is asking for.

---

## Client Review Meeting Prep

**Triggers:** client review, meeting prep, quarterly review, prep for [client name], client meeting

1. Gather client context: household members, account types, total AUM, Investment Policy Statement (IPS) target allocation, risk tolerance, life stage, last meeting date, outstanding action items
2. Pull portfolio performance for each account and the household aggregate:

| Metric | QTD | YTD | 1-Year | 3-Year | Since Inception |
|--------|-----|-----|--------|--------|----------------|
| Portfolio return | | | | | |
| Benchmark return | | | | | |
| Alpha | | | | | |

   - Top 3 contributors and detractors; any outsized single-position impact
3. Drift analysis — current vs. IPS target allocation; flag any drift exceeding ±3–5%
4. Build meeting agenda:
   - Market overview (2–3 min)
   - Portfolio performance and attribution (5 min)
   - Allocation review and rebalancing needs (5 min)
   - Planning updates — life changes, income needs, tax situation, estate (5–10 min)
   - Action items (5 min)
5. Proactive recommendations: rebalancing trades, TLH opportunities, Roth conversion, cash deployment, beneficiary updates, insurance review
6. **Output:** one-page meeting summary (Word/PDF), performance table, allocation chart, action items list, meeting agenda

**Notes:** Know the client before walking in — review prior meeting notes. Lead with what the client cares about. Address bad performance directly. End with clear action items and dates.

---

## Client Performance Report

**Triggers:** client report, performance report, quarterly report, client statement, generate reports

1. Confirm: client name, reporting period (quarter/YTD/annual), accounts in scope, benchmark (from IPS — not whatever looks best), firm branding requirements
2. Build household performance summary table (QTD, YTD, 1-Year, 3-Year ann., 5-Year ann., ITD ann.) — portfolio vs. benchmark vs. alpha
3. Per-account breakdown: account type, value, QTD return, benchmark
4. Allocation overview: current % vs. benchmark %; asset class, $ value
5. Holdings detail: security, asset class, shares, price, value, % of portfolio, QTD return
6. Market commentary: what happened this quarter (2–3 sentences), how it affected the portfolio, outlook — match sophistication level to the client
7. Activity summary: trades, contributions/withdrawals, dividends/interest, fees, rebalancing
8. Planning notes: goal progress, recommendations, upcoming action items, next review date
9. **Output:** PDF report (8–12 pages) — cover page, executive summary, performance, allocation, holdings, commentary, activity, planning notes, disclosures

**Notes:** Performance must be net of fees unless compliance requires gross. Use the benchmark from the IPS. Always include disclaimers. Match detail level to the client — some want every holding, others want one page.

---

## Financial Plan

**Triggers:** financial plan, retirement plan, can I retire, education funding, estate plan, cash flow analysis, plan update

**Step 1 — Client profile:** age/spouse age/dependents, employment and income, all account balances and allocations, income sources (salary, SS estimates, pensions, rental), annual expenses, liabilities, insurance, estate documents

**Step 2 — Cash flow analysis:** build annual projections (gross income → taxes → living expenses → savings → net cash flow); inflation assumption 2.5–3%; model savings directed to pre-tax, Roth, and taxable accounts

**Step 3 — Retirement projections:**
- *Accumulation:* current portfolio + annual contributions + return assumptions → Monte Carlo probability of success at various spending levels
- *Distribution:* inflation-adjusted spending need, Social Security start age (model 62/67/70), RMDs, sustainable withdrawal rate
- Target >85% probability of not running out of money

**Step 4 — Goal-specific analysis:**
- *Education:* 529 balances, target college start, required monthly savings, financial aid considerations
- *Estate:* estate value, tax exposure (federal + state), trust structures, gifting strategy (annual exclusion, lifetime exemption), charitable giving, beneficiary review
- *Risk management:* life insurance needs analysis (income replacement + debt + education), disability adequacy, LTC planning, umbrella liability

**Step 5 — Scenario modeling:** base case, retire 2 years early, 20% market drop in Year 1, higher spending (+20%), one spouse lives to 95, long-term care event — show probability of success for each

**Step 6 — Recommendations:** savings rate changes, allocation adjustments, tax optimization (Roth conversions, TLH, asset location), insurance gaps, estate document updates, beneficiary review

**Output:** financial plan document (15–25 pages), cash flow projection spreadsheet, retirement charts, scenario comparison table, action checklist

**Notes:** Be conservative with return assumptions. Tax planning is as important as investment returns — model tax impact of every recommendation. Always stress-test the plan.

---

## Investment Proposal

**Triggers:** investment proposal, prospect presentation, pitch new client, proposal for [client], new client presentation

1. Gather prospect context: household, current advisor situation, estimated AUM and account types, goals (retirement/growth/income/estate), risk tolerance, constraints (ESG, concentrated stock), current fees, who else they're considering
2. Build proposal with these sections:
   - **Firm overview** (1 page): philosophy, AUM, team, service model
   - **Understanding your needs** (1 page): restate their goals and concerns, show you listened
   - **Proposed strategy** (2–3 pages): recommended allocation with rationale, investment vehicles, tax-aware approach

     | Asset Class | Allocation | Vehicle | Rationale |
     |------------|-----------|---------|-----------|

   - **Expected outcomes** (1–2 pages): projected growth scenarios, Monte Carlo probability of meeting goals, income projections, risk metrics, comparison to current portfolio
   - **Fee structure** (1 page): advisory fee schedule, underlying fund expenses, total all-in cost, value proposition
   - **Getting started** (1 page): account opening process, transfer timeline, transition plan, first 90 days, required documents
3. Customize tone to the prospect type (executive, small business owner, retiree); address concentrated stock or price sensitivity directly
4. **Output:** PowerPoint (12–15 slides) with branding, PDF leave-behind, one-page email summary

**Notes:** Feel personalized — reference their specific situation. Don't oversell performance. Address transition friction directly. Compliance must review before presenting. Follow up within 48 hours.

---

## Portfolio Rebalance

**Triggers:** rebalance, portfolio drift, allocation check, rebalancing trades, out of balance

1. For each account capture: account type (taxable/IRA/Roth/401k), holdings with current market value, cost basis, unrealized gains/losses
2. Drift analysis — compare current to IPS target:

| Asset Class | Target % | Current % | Drift | $ Over/Under |
|------------|----------|-----------|-------|-------------|
| US Large Cap | | | | |
| US Small/Mid Cap | | | | |
| International Developed | | | | |
| Emerging Markets | | | | |
| Investment Grade Bonds | | | | |
| High Yield / Credit | | | | |
| TIPS | | | | |
| Alternatives | | | | |
| Cash | | | | |

   Flag positions exceeding ±3–5% band.
3. Generate trade list with tax-aware rules:
   - Rebalance in tax-advantaged accounts first (no tax consequences)
   - In taxable accounts, avoid selling large short-term gains; harvest losses where possible
   - Watch wash sale rules (30-day window) across all accounts
   - Direct new contributions to underweight classes before trading

   | Account | Action | Security | Shares/$ | Reason | Tax Impact |
   |---------|--------|----------|----------|--------|-----------|

4. Asset location review: bonds/REITs/high-turnover in IRA; highest-growth assets in Roth; tax-efficient equity and TLH candidates in taxable
5. **Output:** drift analysis table, trade list (Excel), tax impact summary, before/after allocation

**Notes:** Don't rebalance within bands — tax costs can outweigh benefits in taxable accounts. Check pending cash flows (contributions, RMDs) before trading. Document rationale for every trade.

---

## Tax-Loss Harvesting

**Triggers:** tax-loss harvesting, TLH, harvest losses, tax losses, unrealized losses, year-end tax planning

1. Scan taxable accounts for unrealized losses; prioritize: largest absolute loss → short-term losses first (offset ordinary income) → largest % loss

| Security | Asset Class | Cost Basis | Current Value | Unrealized Loss | Holding Period | % Loss |
|----------|-----------|-----------|---------------|-----------------|---------------|--------|

2. Calculate gain/loss budget: realized ST gains YTD, realized LT gains YTD, realized losses YTD, net position, carryforward losses → target harvesting amount
   - Tax savings = ST losses × marginal ordinary rate + LT losses × cap gains rate
   - Up to $3,000 net loss deductible against ordinary income; excess carries forward

3. For each harvest candidate, identify a replacement security that:
   - Maintains similar market exposure (same asset class/sector/geography)
   - Is NOT substantially identical (wash sale rule)
   - Has similar risk/return profile

   Common pairs: SPY → IVV, VXUS → ACWX, individual stock → sector ETF

4. Wash sale check — before executing, verify across ALL household accounts (including IRAs and spouse accounts):
   - 30-day lookback: any purchase of substantially identical securities in the past 30 days?
   - 30-day forward block: no repurchase of sold security for 30 days
   - Check DRIPs that could trigger wash sales
   - Document wash sale window for each trade

5. Build execution plan:

| Trade # | Account | Action | Security | Shares | Est. Loss | Replacement | Notes |
|---------|---------|--------|----------|--------|-----------|-------------|-------|

6. Post-harvest: after 30+ days, optionally swap back to original; update cost basis records; document for tax reporting

**Output:** harvest opportunity list (Excel), trade execution sheet, wash sale tracking calendar, tax savings estimate, replacement security rationale

**Notes:** Wash sale violations disallow the loss AND adjust cost basis. Substantially identical ≠ same asset class — ETFs tracking different indexes are generally fine. Document everything. Not all losses are worth harvesting — weigh transaction costs and tracking error.
