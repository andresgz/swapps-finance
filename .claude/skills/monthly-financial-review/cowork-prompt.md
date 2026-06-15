# Monthly Financial Review — Cowork scheduled-task prompt

Paste everything below the `---` into a **Claude Desktop → Cowork → Scheduled Task**, monthly
(e.g. the 1st of each month, to review the month that just closed).
Requires the `quickbooks` (read-only) and `jeeves` MCP servers, plus the `clickup` MCP.
Runs read-only: it only *reads* finance data and *writes one ClickUp message*. It never moves money.

---

You are generating the **Monthly Financial Review for Swapps** (strategic, month-end) and posting it to ClickUp. Work read-only: do NOT create/update/delete anything in QuickBooks or Jeeves. The only write you make is one ClickUp chat message at the end.

Audience is a NON-financial reader. **Keep the abbreviation AND add its plain-Spanish meaning beside it**, e.g. "P&L — ingresos menos gastos", "margen neto (net margin) — % de cada dólar de ingreso que queda como ganancia", "runway — meses que puedes operar con la caja actual". Never leave a term unexplained.

Report the **previous full calendar month** — the month immediately before today's month. This is designed to run on the 1st of each month: e.g. run on Jun 1 → report **May 1–31**. Even if run mid-month, ALWAYS report the previous full calendar month (never the partial current month). Always include **year-to-date** context and a **multi-month trend**. Use markdown tables. This is the strategic monthly view — focus on trends, customer/vendor concentration, and structural decisions (the weekly pulse already covers operational week-to-week detail).

Compute the period explicitly: target month = the calendar month before today. start_date = first day of that month, end_date = last day of that month. Aging reports (A/R, A/P) use report_date = last day of that month.

## Data to pull
1. **P&L trend (QuickBooks):** `get_profit_and_loss` for the year with `summarize_column_by: "Month"` — build a month-by-month table of Ingresos, Gastos, Ganancia neta, Margen neto %.
2. **Month + YTD P&L:** `get_profit_and_loss` for the target month and for year-to-date (Jan 1→today) — income, full expense breakdown by account, net income.
3. **Revenue by customer:** `get_customer_sales` (target month and/or YTD) — show concentration (top clients, % of total).
4. **Top vendors / expenses:** `get_vendor_expenses` — largest vendors for the month/YTD.
5. **Cash flow:** `get_cash_flow` for the month; plus `get_balance_sheet` today for cash per account and partner-distribution/equity figures.
6. **A/R & A/P:** `get_aged_receivables` and `get_aged_payables` — summary totals + anything >60 días.
7. **Subscription/SaaS audit (Jeeves):** `list_cards` (active) and `list_transactions` (last 30–60 días). Identify recurring software charges, total monthly software spend, likely **duplicate** cards (e.g. "X" vs "X 2026"), and unused/near-zero cards.

## Output format (markdown, Spanish, term + meaning, tables)
Title: `📊 **Revisión Financiera Mensual Swapps — <mes YYYY>**`
Sections:
1. `🧭 **Resumen ejecutivo**` — headline numbers (ingresos, gastos, ganancia neta, margen neto %, caja total, runway) + top 🔴/🟡 flags.
2. `📈 **Tendencia mensual (P&L)**` — TABLE: Mes | Ingresos | Gastos | Ganancia neta | Margen % (last ~6 months). One line on the trend.
3. `👥 **Ingresos por cliente (concentración)**` — TABLE: Cliente | Monto | % del total. Flag if one client is a large share.
4. `💸 **Gastos por categoría + principales proveedores**` — TABLE of expense accounts; TABLE of top vendors.
5. `🧾 **Auditoría de suscripciones / software**` — total mensual de software; TABLE of recurring SaaS; call out duplicate/unused cards and estimated savings.
6. `💰 **Flujo de caja y runway**` — entradas vs salidas del mes, saldo por cuenta, distribución a socios (partner distributions), runway.
7. `📥📤 **Cuentas por Cobrar (A/R) y por Pagar (A/P)**` — summary totals + anything >60 días.
8. `💡 **Recomendaciones**` — optimization (spend, subscriptions) + chart-of-accounts adjustments (only new/unresolved).
Footer: `_Solo lectura · generado automáticamente · revisión mensual_`

Use exact dollar figures. Explain terms inline. Keep it scannable.

## Deliver
Send with `clickup_send_chat_message`:
- `channel_id`: `5-90147717252-8`  (the "Finance" channel)
- `content_format`: `text/md`
- `content`: the full report.
Confirm the message_id in your final reply.
