# Books Health Check — Sandbox Company US 036e
Run date: 2026-11-23 · Source: QuickBooks Online via API/MCP (read-only)
Scale: 89 accounts · 26 vendors · 29 customers · Fiscal year starts January

## Summary
Sample-data company, so treat this as a format proof, not a real engagement. The books tie
out (AR and AP control accounts match their open-item totals exactly), but they carry the
two classic cleanup findings: Opening Balance Equity never cleared ($9,337.50) and money
sitting in Undeposited Funds ($2,062.52). Receivables need attention: 10 of 20 open
invoices are overdue.

## Findings

### High
| # | Finding | Amount / count | Why it matters | Fix |
|---|---------|----------------|----------------|-----|
| 1 | Opening Balance Equity not zero | $9,337.50 | Setup entries were never classified, so equity is misstated and every balance sheet since carries the error | Trace the opening entries and reclassify to retained earnings / owner equity with the accountant |
| 2 | Overdue receivables | $1,525.50 across 10 invoices; oldest due 2026-09-19 | Cash is sitting in customers' hands; oldest is 2+ months past due | Work the list top-down: John Melton $450.00, Barnett Design $274.50, Amy's Bird Sanctuary $239.00 |

### Medium
| # | Finding | Amount / count | Why it matters | Fix |
|---|---------|----------------|----------------|-----|
| 1 | Undeposited Funds not clearing | $2,062.52 | Payments recorded but never deposited in QBO risks duplicate income when the bank feed deposit also posts | Match each undeposited payment to its bank deposit and clear |
| 2 | Overdue payables | $847.67 across 4 bills; oldest due 2026-10-13 | Late vendor payments strain supplier relationships and can incur fees | Schedule payment for Norton Lumber, Robertson & Associates, PG&E bills |

### Low
| # | Finding | Amount / count | Why it matters | Fix |
|---|---------|----------------|----------------|-----|
| 1 | Generic accounts still active | "Miscellaneous" (Other Expense), $0 balance | Invites miscoding at entry time | Inactivate or rename with a specific purpose |

## Checked and clean
- AR control ($5,281.52) ties exactly to open invoices; AP control ($1,852.67) ties to open bills.
- No duplicate vendors or customers (26 / 29 records, normalized-name comparison).
- No negative bank accounts. Uncategorized Income/Expense/Asset accounts all at $0.
- Note: open bills include a $250 bill from Bob's Burger Joint created 2026-11-23 as an
  integration smoke test — real data in the sandbox, flagged here for transparency.

## Fix first
1. Clear Opening Balance Equity — it distorts equity on every report until it's gone.
2. Clear Undeposited Funds — removes the duplicate-income risk.
3. Collect the top 3 overdue invoices — $963.50 of the $1,525.50 total.

## Could not verify
- Report endpoints (P&L / balance sheet / aging) returned a permission error from the
  automation host used for this run, so aging was computed from open-item queries instead.
  From the operator's own machine the reports load normally (verified 2026-11-23).
