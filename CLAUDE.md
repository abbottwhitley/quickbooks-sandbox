# Working in this repo

This repo holds playbooks for AI-assisted QuickBooks Online work, executed through Intuit's
official `quickbooks-online-mcp-server` connected as the `quickbooks` MCP server.

Rules:
- **Read-only by default.** Never create, update, or delete anything in a client's books
  unless the human operator explicitly asks for that specific write in this session.
  Audit playbooks never write.
- **Approvals live in QuickBooks.** Anything consequential (paying a bill, sending an
  invoice) is drafted at most; the human approves in QBO, where the audit trail is.
- **Report what you actually found.** Every figure in a report must come from a tool result
  in this session — never from memory of a previous run or a "typical" value. If a check
  could not be run, say so in the report instead of guessing.
- Playbooks live in their own folders with a `PLAYBOOK.md` (the checks, in order) and a
  `report-template.md` (the exact output shape). Follow the template exactly.
