# QuickBooks Toolkit

Working tools for AI-assisted QuickBooks Online engagements — built around Intuit's official
`quickbooks-online-mcp-server` (https://github.com/intuit/quickbooks-online-mcp-server), which
exposes the full QBO API (~145 tools) to AI clients like Claude Code and Claude Desktop.

Intuit's built-in AI and its bundled Claude connector cover asking questions about one
company's books. This toolkit is the layer around that: firm-wide sweeps, custom rules,
cleanup audits, and scheduled packs — across systems and across a book of clients.

## Contents

- `health-check/` — **Books Health Check**: a read-only cleanup audit playbook. Scans a QBO
  company for the findings a bookkeeper looks for before taking on or cleaning up a set of
  books, and produces a one-page findings report.
- `reports/` — sample output (generated against Intuit's sandbox sample company).

## Setup

1. Install and configure the MCP server per its README (developer app, OAuth, `.env` with
   realm ID + refresh token). Point it at the company you want to audit.
2. For audits, run it read-only: set `QUICKBOOKS_DISABLE_WRITE=true`,
   `QUICKBOOKS_DISABLE_UPDATE=true`, `QUICKBOOKS_DISABLE_DELETE=true` in the server's `.env`.
   The Health Check never writes.
3. Open this repo in Claude Code (or any MCP-capable client) with the server connected,
   and ask it to run the playbook in `health-check/PLAYBOOK.md`.

## Status

First playbook built and verified against Intuit's sandbox company (Sandbox Company US 036e),
November 2026. Sandbox only so far; production use needs Intuit's app assessment and a
per-client authorization flow.
