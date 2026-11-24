# AP Prep Pack — Playbook

Turn a pile of supplier invoices (email text, PDFs, scans — anything the operator drops in)
into **draft bills in QuickBooks, queued for human approval**, plus a one-page review pack
explaining every decision. The AI prepares; the human approves in QBO, where the audit
trail lives. This playbook never pays a bill and never sends anything.

Why this exists: Intuit's bundled Claude connector deliberately has no bill creation, and
QBO's own email-to-bill handles one clean invoice at a time with no judgment. The value
here is the judgment layer in the middle: matching, duplicate detection, categorization,
and honest exceptions.

## Inputs
- A batch of invoices in any text-readable form (samples in `samples/`).
- The connected `quickbooks` MCP server pointed at the target company.

## Step 1 — Read the batch
For each invoice extract: vendor name (as written), invoice number, invoice date, due date
(if stated), total, and line items with amounts. If a field is missing or two readings of
the total disagree, mark the invoice **unclear** now — it will become an exception, not a guess.

## Step 2 — Load the ground truth from QBO
- All vendors: `SELECT Id, DisplayName FROM Vendor`
- All expense/COGS accounts: `SELECT Id, Name, AccountType FROM Account WHERE AccountType IN ('Expense','Cost of Goods Sold','Other Expense')`
- Open bills: `SELECT Id, DocNumber, VendorRef, TxnDate, DueDate, Balance, TotalAmt FROM Bill WHERE Balance > '0'`
- Recent bills regardless of status (duplicate window, last 90 days) if the tool allows.

## Step 3 — Match the vendor
Normalize names (case, punctuation, "Co."/"Company", legal suffixes). Outcomes:
- **Exact/confident match** → use that vendor.
- **No match** → NEW VENDOR exception. Do not silently create vendors in a real company's
  books. (In a sandbox or with explicit operator approval, create it and flag it in the
  pack as created-pending-review.)
- **Ambiguous** (two plausible vendors) → exception.

## Step 4 — Duplicate check (before drafting anything)
An invoice is a **suspected duplicate** if an existing bill has the same vendor AND
(the same invoice/DocNumber OR the same total within ±$0.01 and a date within 45 days).
Suspected duplicates are never drafted. They go to the pack as exceptions with the
existing bill's ID cited — the human decides if it's a re-send or a real second charge.

## Step 5 — Categorize
Assign each line to an account from the company's own chart, using the vendor's history
first (what account did this vendor's last bills use?) and the line description second.
If the best category is a guess, say so in the pack ("categorized as X — low confidence").

## Step 6 — Draft the bills
For every invoice that is matched, non-duplicate, and fully read:
- Create the bill (`create-bill`) with VendorRef, DocNumber (vendor's invoice number),
  TxnDate, DueDate (from the invoice; if unstated, invoice date + vendor's usual terms,
  flagged), and AccountBasedExpense lines.
- Record the created bill ID for the pack. Drafts only — nothing is paid or sent.

## Step 7 — The review pack
Produce the pack using the shape in a health-check-style summary:
- **Drafted** table: invoice → vendor, amount, due date, category, new QBO bill ID.
- **Exceptions** table: invoice → reason (new vendor / suspected duplicate of bill #N /
  unclear total / missing due date) and the exact decision the human needs to make.
- **Totals**: batch total, drafted total, exception total.
- One line at the top: "Nothing here is paid. Approve or delete each draft in QuickBooks."

## Guardrails
- A total you cannot read cleanly is an exception, never a drafted number.
- Duplicate suspicion beats drafting speed, every time.
- Every figure in the pack comes from this run's tool results.
