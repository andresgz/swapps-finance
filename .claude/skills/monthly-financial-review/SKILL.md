---
name: monthly-financial-review
description: Generate the Swapps monthly strategic financial review — P&L trend & margin over the last months, revenue by customer (concentration), expense breakdown & top vendors, SaaS/subscription audit (from Jeeves cards), cash flow & runway, partner distributions, and recommendations — then post it to the ClickUp "Finance" channel. Use when asked for the monthly financial review, "revisión financiera mensual", or a strategic/month-end finance report. The strategic counterpart to weekly-financial-pulse. Requires the quickbooks (read-only), jeeves, and clickup MCP connections.
---

# Monthly Financial Review

The complete, authoritative spec lives in **`cowork-prompt.md`** in this same folder — it
defines exactly what to pull (QuickBooks + Jeeves), the section order, the plain-language
style, and the ClickUp delivery target. **Follow that file as the single source of truth**
(the Cowork scheduled task uses the identical spec). Read it, then execute it.

## Non-negotiables
- **Read-only.** Never create/update/delete in QuickBooks or Jeeves. The only write is one
  ClickUp chat message.
- **Deliver** to the ClickUp "Finance" channel — `channel_id 5-90147717252-8` — via
  `clickup_send_chat_message`, `content_format: text/md`. Report the returned `message_id`.
- **Audience is non-financial:** keep each abbreviation AND its plain-Spanish meaning beside
  it (e.g. "margen neto (net margin) — qué % de cada peso de ingreso queda como ganancia").
- This is **monthly/strategic** — focus on trends, concentration, and structural decisions,
  not the week-to-week operational detail that `weekly-financial-pulse` already covers.

## Behavior
- Report the **previous full calendar month** — the month before the current one (designed to run on the 1st: e.g. run on June 1 → report May). Even if run mid-month, always the previous full month, never the partial current month. Include year-to-date context and a multi-month trend.
- If the user asks to preview, show the report in chat and post only after approval.
- If a connection is missing or a token expired, say so plainly and stop — do not invent figures.
