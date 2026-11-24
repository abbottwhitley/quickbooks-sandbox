# AP Review Pack — Sandbox Company US 036e

**Run:** November 23, 2026 · **Batch:** 6 supplier invoices · **Prepared by:** AP Prep Pack playbook (read batch → match vendors → duplicate check → categorize → draft)

> Nothing here is paid. Approve or delete each draft in QuickBooks — approval lives in QBO, where the audit trail is.

## Drafted in QuickBooks (4 bills, $2,355.92)

| Invoice | Vendor | Amount | Due | Category | QBO bill |
|---|---|---:|---|---|---|
| NL-4471 (Nov 20) | Norton Lumber and Building Materials | $1,240.00 | Dec 4, 2026 | Job Materials | #146 |
| PG&E statement (Nov 18) | PG&E | $186.42 | Dec 9, 2026 | Utilities | #147 |
| ER-8812 (Nov 21) | Ellis Equipment Rental | $540.00 | Dec 21, 2026 ⚠ | Equipment Rental | #148 |
| RMM-1043 (Nov 22) | Rocky Mountain Mulch Co. ⚠ | $389.50 | Dec 6, 2026 | Plants and Soil | #149 |

⚠ Flags on drafts:
- **Ellis (#148):** the invoice stated no due date. Drafted as Net 30 (Dec 21) from "payment terms on file" — confirm the real terms before approving. Also note: this chart has **two** accounts named "Equipment Rental" (IDs 29 and 62); drafted to 29. Worth consolidating.
- **Rocky Mountain Mulch (#149):** **new vendor** — not in QuickBooks before this run. Created (vendor ID 200000001) and drafted because this is a sandbox; in a live company's books this stops at the exception list until a human approves the new vendor.

## Exceptions — need a human decision (2 invoices)

| Invoice | Amount | Why it's held | Decision needed |
|---|---:|---|---|
| Robertson & Associates (Oct 27) | $315.00 | **Suspected duplicate** of existing bill **#108** — same vendor, same $315.00 total, dated Oct 28, 2026, still open. The sender's own note says "also sent last month." | Confirm it's a re-send and ignore, or if it's a genuine second charge, say so and it will be drafted. |
| Hicks Hardware receipt (Nov 19) | $212.18 or $221.18 | **Total is unreadable** — the scan is smudged and the two readings disagree ($212.18 subtotal vs. a total that reads as either $221.18 or $212.18). Never draft a guessed number. | Get a clean copy or confirm the total with the vendor. |

## Totals
- Batch total: **$2,882.92** drafted + held (using $212.18 for the unreadable receipt)
- Drafted: **$2,355.92** (4 bills)
- Held for decisions: **$527.00** (2 invoices)

## How this ran
Invoices were read from plain-text samples (`ap-prep-pack/samples/`), matched against the live vendor list (26 vendors), checked against all 16 existing bills, categorized from the company's own chart of accounts, and drafted through the QuickBooks API. Every figure above comes from this run's reads and the read-back verification of the four created bills.
