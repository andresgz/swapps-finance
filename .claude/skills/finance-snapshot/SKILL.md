---
name: finance-snapshot
description: On-demand financial status for Swapps — current cash by account, P&L (ganancias y pérdidas) month-to-date and year-to-date, Cuentas por Cobrar (A/R) and Cuentas por Pagar (A/P) aging, plus any alerts. Answers in chat and does NOT post to ClickUp unless explicitly asked. Use for ad-hoc questions like "¿cómo vamos?", "estado financiero", "cuánta caja tenemos", "cómo va el mes". Requires the quickbooks (read-only) and jeeves connections.
---

# Finance Snapshot (on-demand)

Read-only. Give a quick, accurate financial status from the live connections. This is the
lightweight ad-hoc counterpart to `weekly-financial-pulse` (which is the full scheduled report).

## Pull and summarize
1. **Cash** — `get_balance_sheet` (QuickBooks), today: balance per bank account + total. Optionally
   `list_accounts` (Jeeves) for live spend-account balances.
2. **P&L** — `get_profit_and_loss` month-to-date (1st of month→today) and year-to-date
   (Jan 1→today): income, total expenses, net income.
3. **Cuentas por Cobrar (A/R)** — `get_aged_receivables` (today): who owes us; flag anything >30 días vencido.
4. **Cuentas por Pagar (A/P)** — `get_aged_payables` (today): what we owe; flag overdue / due soon.

## Style
- Concise, with small markdown tables. Lead with any 🔴/🟡 alerts.
- Term + plain-Spanish meaning beside each abbreviation (audience is non-financial), e.g.
  "Cuentas por Cobrar (A/R) — lo que te deben", "runway — meses de operación".
- **Read-only.** Never move money. Do NOT post to ClickUp unless the user explicitly asks
  (if they do, use the Finance channel `5-90147717252-8`).
- Scale the depth to the question — a "cuánta caja tenemos" only needs the cash table.
