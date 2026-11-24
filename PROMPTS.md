# Template prompts — run these yourself

Setup assumed for all of them: this repo cloned locally, Claude Code (or Cline / Claude
Desktop) open in the repo folder, and the QuickBooks MCP server connected to the company
you want to work on. The playbooks hold the instructions — the prompt just points at them.

## Books Health Check (read-only)

```
Run the Books Health Check in health-check/PLAYBOOK.md against the connected
QuickBooks company. Read-only — change nothing. Write the report using
health-check/report-template.md and save it under reports/ with today's date.
```

## AP Prep Pack (drafts bills for approval — writes drafts only)

```
Run the AP Prep Pack in ap-prep-pack/PLAYBOOK.md. The invoices to process are the
files in ap-prep-pack/samples/ (or: the files in <folder> / the invoices I paste
below). Draft bills in QuickBooks for the clean ones, hold anything that is a
suspected duplicate, a new vendor, or unreadable as an exception, and give me the
review pack at the end. Never pay or send anything.
```

## Variations worth knowing

- **Point it at real intake:** replace the samples line with "the invoice emails from
  the last 7 days in my inbox" (needs an email tool connected) or a folder of PDFs —
  the playbook's Step 1 is source-agnostic.
- **One company → many:** for a firm, run the same prompt once per connected company,
  or ask for a sweep once each company is connected in turn.
- **Tighten the rules:** thresholds and exceptions live in the playbooks, not the
  prompts — edit the playbook (e.g. duplicate window, assumed payment terms) and every
  future run follows it.
