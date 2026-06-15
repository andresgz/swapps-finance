---
name: weekly-financial-pulse
description: Generate the Swapps weekly financial report — cash by account, P&L, Cuentas por Cobrar (A/R), Cuentas por Pagar (A/P), Jeeves spend, Jeeves↔QuickBooks reconciliation, and recommendations — covering the week + month-to-date + year-to-date, then post it to the ClickUp "Finance" channel. Use when asked for the weekly financial pulse, "pulso financiero semanal", or a recurring finance report. Requires the quickbooks (read-only), jeeves, and clickup MCP connections.
---

# Weekly Financial Pulse

The complete, authoritative spec lives in **`cowork-prompt.md`** in this same folder — it
defines exactly what data to pull (QuickBooks + Jeeves), the severity thresholds, the
section order, the plain-language style, and the ClickUp delivery target. **Follow that file
as the single source of truth** (the Cowork scheduled task uses the identical spec, so they
never drift). Read it, then execute it.

## Non-negotiables
- **Read-only.** Never create/update/delete anything in QuickBooks or Jeeves. The only write
  is one ClickUp chat message at the end.
- **Deliver** to the ClickUp "Finance" channel — `channel_id 5-90147717252-8` — with
  `clickup_send_chat_message`, `content_format: text/md`. Report the returned `message_id`.
- **Audience is non-financial:** keep every abbreviation AND its plain-Spanish meaning beside
  it (e.g. "Cuentas por Cobrar (A/R) — lo que te deben"). Detailed markdown tables; list
  detail rather than summarizing it away.

## Behavior
- If the user asks to preview, show the report in chat and post only after approval.
- If a connection is missing or a token expired, say so plainly and stop — do not invent figures.
