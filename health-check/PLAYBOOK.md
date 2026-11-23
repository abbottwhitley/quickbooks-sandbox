# Books Health Check — Playbook

A read-only cleanup audit of one QuickBooks Online company. Run every check, in order,
through the connected `quickbooks` MCP server. Then produce the report using
`report-template.md` — every number from this run's tool results, nothing remembered.

Severity guide: **High** = distorts the financial statements or hides money. **Medium** =
will cause pain at close, tax time, or handoff. **Low** = hygiene.

## Check 1 — Opening Balance Equity is not zero (High)
Opening Balance Equity should wash to $0 once a company's books are set up. A lingering
balance means setup entries never got classified — the classic cleanup finding.
- Get the balance sheet (`get_balance_sheet`) or query the account:
  `SELECT Id, Name, CurrentBalance FROM Account WHERE Name LIKE '%Opening Balance Equity%'`
- Record the balance. Non-zero = finding.

## Check 2 — Undeposited Funds is stale (Medium)
Money sitting in Undeposited Funds should clear to the bank within days. An old or large
balance usually means duplicate income: payments recorded but never deposited in QBO,
while the real deposit also hit the bank feed.
- Query the account balance as above (`Name LIKE '%Undeposited Funds%'`).
- If non-trivial, pull recent payments (`search_payments`) and note the oldest undeposited item you can identify.

## Check 3 — Duplicate vendors (Medium)
Duplicates split a vendor's history and 1099 totals.
- `SELECT Id, DisplayName FROM Vendor` (all). Compare normalized names (case/punctuation
  insensitive; watch for "Co." vs "Company", trailing spaces, "The " prefixes).
- List each suspected duplicate pair.

## Check 4 — Duplicate customers (Medium)
Same as Check 3, against `Customer`. Duplicates split AR history and age balances wrong.

## Check 5 — Dormant accounts still active (Low)
Accounts with no activity in 12+ months clutter the chart and invite miscoding.
- `SELECT Id, Name, AccountType, CurrentBalance FROM Account WHERE Active = true`
- For balance-sheet accounts, a balance that never moves is the signal you can get from
  the API; flag active accounts with zero balance and generic names ("Miscellaneous",
  "Ask My Accountant", "Other") for review, and list any "Ask My Accountant" balance (High if non-zero — it's a suspense account).

## Check 6 — Overdue receivables (High if material)
- Get the aged receivables report (`get_aged_receivables`) or query open invoices:
  `SELECT Id, DocNumber, CustomerRef, DueDate, Balance FROM Invoice WHERE Balance > '0'`
- Total overdue, count, oldest due date, and the top 3 customers by overdue balance.

## Check 7 — Overdue payables (Medium)
- Same pattern for bills:
  `SELECT Id, DocNumber, VendorRef, DueDate, Balance FROM Bill WHERE Balance > '0'`
- Total overdue, count, oldest. (Sandbox note: a bill created during setup/testing may
  appear here — that's real data, report it.)

## Check 8 — Uncategorized / suspense activity (Medium)
- Look for accounts named "Uncategorized Expense", "Uncategorized Income",
  "Uncategorized Asset" with non-zero balances (query as in Check 1).
- Non-zero = transactions nobody has reviewed.

## Check 9 — Negative or impossible balances (High)
- From the full account list (Check 5), flag: bank accounts negative, credit card accounts
  with a debit (positive asset-style) balance, AR or AP control accounts whose balance
  doesn't match the aging totals from Checks 6–7 (report both numbers if they differ).

## Check 10 — Company snapshot (context, not a finding)
- `get_company_info`: legal name, country, fiscal year start. Note the QBO plan/SKU if visible.
- Count of accounts, vendors, customers — gives the report its scale line.

## Output
Fill `report-template.md`. Order findings by severity, then by dollar impact. For each
finding: what it is, the number, why it matters in one sentence, and the fix in one
sentence. End with the three things you'd fix first. If a check failed (permissions,
missing report), list it under "Could not verify" — never silently drop it.
