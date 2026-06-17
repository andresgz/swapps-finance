# Weekly Financial Pulse — Cowork scheduled-task prompt

Paste everything below the `---` into a **Claude Desktop → Cowork → Scheduled Task**, weekly.
Requires the `quickbooks` (read-only), `jeeves` and `swapps-app` (read-only) MCP servers, plus the `clickup` MCP.
Runs read-only: it only *reads* finance data and *writes one ClickUp message*. It never moves money.

---

You are generating the **Weekly Financial Pulse for Swapps** and posting it to ClickUp. Work read-only: do NOT create/update/delete anything in QuickBooks, Jeeves or swapps-app. The only write you make is one ClickUp chat message at the end.

Audience is a NON-financial reader. **Keep the standard abbreviation AND add its plain-Spanish meaning beside it**, e.g. "Cuentas por Cobrar (A/R) — lo que te deben", "Cuentas por Pagar (A/P) — lo que debes", "runway — meses que puedes operar con la caja actual", "P&L — ingresos menos gastos". Never drop the abbreviation, and never leave it unexplained.

Be DETAILED and use **markdown tables** everywhere. Consolidate at the top, but also show the underlying detail: every bank account, every A/R customer and A/P vendor by age bucket, and every individual expense of the week. Cover THREE time windows: the **week** (last 7 days), the **month to date** (1st of current month → today), and **year to date** (Jan 1 → today).

Use today's date as the report date.

## Data to pull
1. **Cash (QuickBooks):** `get_balance_sheet` as of today AND 7 days ago. Every bank account + total, with week-over-week change.
2. **P&L (QuickBooks):** `get_profit_and_loss` for month-to-date (1st of month→today) AND year-to-date (Jan 1→today). Capture income, total expenses, net income, and the largest expense accounts.
3. **A/R (QuickBooks):** `get_aged_receivables` (Report_Date, today) — every customer by bucket.
4. **A/P (QuickBooks):** `get_aged_payables` (Report_Date, today) — every vendor by bucket; note anything due ≤7 days.
5. **Jeeves spend (Jeeves):** `list_transactions` startDate = 1st of current month. Itemize every CARD charge of the **last 7 days** (date, source, merchant, category, amount); summarize the **month-to-date** spend by category; list pending payments and cashback.
6. **Reconciliation (Jeeves):** `list_accounts` → live USD; compare to QBO "JEEVES USD 8485421844".
7. **MRR contratado (swapps-app):** `list_contracts` (activos) + `list_contract_services` → sum the committed monthly recurring revenue (MRR — ingreso recurrente mensual comprometido en contratos). Read-only. This is what clients are *contracted* to pay, independent of what's been invoiced in QuickBooks.

## Thresholds → severity
- 🔴 Atender / 🟡 Revisar / 🟢 Bien.
- **Caja:** 🟡 total drops >15% w/w; 🔴 total < $60K or runway < 2 months.
- **A/R:** 🟡 1–30 days overdue; 🔴 >30 days overdue OR any client >$5K overdue.
- **A/P:** 🟡 due within 7 days; 🔴 overdue (flag +90 días).
- **Jeeves charge:** 🟡 new merchant or charge >$300; 🔴 over card limit or unexpected >$1,000.
- **Reconciliation:** 🟡 >$500; 🔴 >$2,000; say how much pending payments explain.

## Output format (markdown, Spanish, term + meaning, detailed tables)
Title: `📊 **Pulso Financiero Swapps — al <DD mmm YYYY>** (semana + mes + año a la fecha)`
Sections in order:
1. `🚦 **Atención (ordenado por urgencia)**` — TABLE: Sev | Tema | Detalle | Acción sugerida. 🔴 then 🟡. If none: "Sin alertas 🟢".
2. `💰 **Caja — saldo de cada cuenta**` (define runway) — TABLE: Cuenta | hace 1 sem | hoy | Δ + TOTAL.
3. `📈 **Resultados (P&L = ingresos − gastos)**` — TABLE: Concepto | Mes a la fecha | Año a la fecha (Ingresos, Gastos, Ganancia neta). Below it, list month expenses by account and the largest YTD expense accounts.
4. `📥 **Cuentas por Cobrar (A/R) — lo que te deben**` — TABLE by customer & bucket + TOTAL.
5. `📤 **Cuentas por Pagar (A/P) — lo que debes**` — TABLE by vendor & bucket + TOTAL; note due ≤7 días.
6. `⏳ **Pagos de Jeeves en proceso (pending)**` — TABLE Fecha | Beneficiario | Monto.
7. `💳 **Detalle de gastos — SEMANA (Jeeves)**` — TABLE per charge (Fecha | Origen | Comercio | Categoría | Monto).
8. `💳 **Gastos — MES a la fecha por categoría**` — TABLE Categoría | Monto + total. State 🟢 si sin anomalías.
9. `🔗 **Conciliación Jeeves ↔ QuickBooks**` — TABLE Fuente | Saldo + diferencia, explained.
10. `📑 **MRR contratado vs facturado**` (MRR — ingreso recurrente mensual comprometido en contratos) — compare the contracted MRR (swapps-app) against the month-to-date invoiced income (QuickBooks P&L). TABLE: MRR contratado | Facturado MTD | Brecha. Flag 🟡/🔴 if there's contracted revenue not being invoiced (under-billing) or a drop vs the prior run that suggests churn. One line interpreting the gap.
11. `💡 **Recomendaciones de optimización de gastos**` — numbered list, based on the largest/recurring/duplicate spend you see this run.
12. `🗂️ **Recomendaciones de ajustes en el plan de cuentas**` — ONLY include new/unresolved classification issues (duplicate accounts, uncategorized balances, mis-classified items, accounts split across categories). Skip items already cleaned up. If nothing new, omit this section.
Footer: `_Solo lectura · generado automáticamente · umbrales y recomendaciones ajustables_`

Use exact dollar figures. Explain any term inline. Keep it scannable.

## Deliver
Send with `clickup_send_chat_message`:
- `channel_id`: `5-90147717252-8`  (the "Finance" channel)
- `content_format`: `text/md`
- `content`: the full report.
Confirm the message_id in your final reply.
