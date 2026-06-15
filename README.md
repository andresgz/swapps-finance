# Swapps — Finance Automation & Context

This repository versions the **Claude skills and operating context** for running financial
analysis on Swapps using live connections to **Jeeves** and **QuickBooks Online**, with
reports delivered to **ClickUp**.

It exists so that any Claude session in this folder immediately knows *what it can access*
and *what it can do* with these connections.

## Connections available

| Connection | Type | Use | Mode |
|---|---|---|---|
| **QuickBooks Online** | Official Intuit MCP (local stdio) | Books of record: P&L, Balance Sheet, Cash Flow, A/R & A/P aging, chart of accounts, invoices, bills, customers, vendors | **READ-ONLY** |
| **Jeeves** | Remote MCP | Corporate spend: cards, transactions, bill-pay, contractor/vendor payments, statements, balances | Read (advisory) |
| **ClickUp** | MCP | Delivery: posts reports to the **"Finance"** chat channel (`id 5-90147717252-8`) | Write (messages only) |

- QuickBooks: company **SWAPPS USA LLC**, realm `123146088475019`. The server lives at
  `quickbooks-online-mcp-server/` (gitignored — external clone + secrets) and is locked
  read-only via `QUICKBOOKS_DISABLE_WRITE/UPDATE/DELETE=true`.
- Re-auth procedure (token lasts ~100 days, auto-rotates): see Claude memory `quickbooks-mcp-setup`.

## What can be done
Cash position & runway · profit/margin trends · collections (A/R) & payables (A/P) aging ·
spend anomaly detection · Jeeves↔QuickBooks reconciliation · expense-optimization and
chart-of-accounts recommendations.

## Skills (`.claude/skills/`)
- **`weekly-financial-pulse`** — full weekly report (week + month-to-date + year-to-date),
  posted to the Finance channel. Also runs as a Cowork scheduled task via its `cowork-prompt.md`.
- **`finance-snapshot`** — on-demand financial status (cash, P&L, A/R, A/P) answered in chat.

Invoke in Claude Code with `/weekly-financial-pulse` or `/finance-snapshot`.

## Guardrails
- All analysis is **read-only** — nothing ever moves money.
- Secrets (`.env`) and the cloned QBO server are **not** versioned (see `.gitignore`).

## Automation
Weekly delivery runs as a **Claude Desktop → Cowork scheduled task** (local, so it can reach
the local QuickBooks server). Paste the contents of
`.claude/skills/weekly-financial-pulse/cowork-prompt.md` into the task.
