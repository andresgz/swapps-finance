# Prompts para las routines de Cowork

Cada reporte tiene su **spec completo** aquí (`weekly-financial-pulse.md`, `monthly-financial-review.md`)
y también como **skill** en `.claude/skills/`. En la routine de Cowork **no hace falta pegar todo el spec** —
basta un prompt corto que apunte al archivo o invoque la skill (Cowork corre local y tiene acceso a ambos).

## ✅ Recomendado: prompt corto (pega esto en la routine)

**Semanal — programa: lunes**
```
Lee el archivo /Users/andres/projects/finance/prompts/weekly-financial-pulse.md y ejecútalo tal cual.
Trabaja en modo solo lectura; la única escritura permitida es publicar el reporte en el canal "Finance" de ClickUp.
```

**Mensual — programa: día 1 de cada mes**
```
Lee el archivo /Users/andres/projects/finance/prompts/monthly-financial-review.md y ejecútalo tal cual,
reportando el mes anterior completo. Solo lectura, salvo el mensaje en el canal "Finance" de ClickUp.
```

## Alternativa aún más corta (si Cowork ve las skills del proyecto)
- Semanal: `Ejecuta la skill weekly-financial-pulse.`
- Mensual: `Ejecuta la skill monthly-financial-review para el mes anterior.`

> Si la skill no se carga (Cowork no siempre descubre skills de proyecto), usa el prompt corto que lee el archivo:
> es igual de breve y siempre funciona.

## Archivos (spec completo)
- `weekly-financial-pulse.md` — reporte semanal (semana + mes + año a la fecha).
- `monthly-financial-review.md` — revisión mensual del mes anterior.

Estos son espejo de `.claude/skills/*/cowork-prompt.md`.
