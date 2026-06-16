# Eaze

Eaze is Profitbase InVision's domain-specific formula language for cell calculations in data grids. It targets cells by column address, evaluates expressions, and writes results back — statement by statement.

A typical formula:

```
@<column>[<row-filter>] = <expression>;
```

```
@Amount[] = Price * Qty;
```

## Where Eaze is used

| Surface | Role |
|---|---|
| Worksheet Calculations | Cell formulas on Data Store columns |
| Worksheet Cell Validation rules | Target + Statement expressions |
| Worksheet Cell Read-Only rules | Target + Statement expressions |
| Worksheet Row/Cell Styling | `SETROWSTYLE` / `SETCELLSTYLE` conditions |
| Table calculations (input-table) | Same as Worksheet |
| SQL Report tabular output | Derived column expressions |
| Workbook action instructions | Parameters typed `boolean \| Eaze expression` |

> **Not Eaze:** Filter / Caption / Header / Is-Hidden / Visibility fields in the Workbook designer use **C# directive expressions** (`Directive(...)`, `Localize(...)`, `@Object[...]`, `@Context.ObjectAlias`) — a different evaluator entirely.

## Language reference

- [Cell Addressing](language/cell-addressing.md) — `@Column[]`, row predicates, cross-sheet `!`, `@Property[]`
- [Keywords and Context](language/keywords-and-context.md) — `true`, `false`, `null`, `this` and its properties
- [Operators](language/operators.md) — arithmetic, comparison, logical, null-coalescing
- [Execution Model](language/execution-model.md) — statement order, recalculation triggers, `RecalcAction`

## Function reference

- [Function index](functions/index.md) — all functions at a glance
- [Logical](functions/logical.md) — `IF`, `ISNULL`, `COALESCE`, `ISERROR`, …
- [Math / Trig](functions/math.md) — `SUM`, `ABS`, `ROUND`, `POW`, …
- [Statistical](functions/statistical.md) — `AVERAGE`, `COUNT`, `MAX`, `STDEV`, …
- [Text](functions/text.md) — `CONCAT`, `SUBSTRING`, `TOSTRING`, …
- [Date](functions/date.md) — `DATE`, `NOW`, `FORMATDATE`, `TODATE`, …
- [Time Frame column helpers](functions/timeframe-dates.md) — `YearNum()`, `MonthNum()`, `WeekNum()`, …
- [Financial](functions/financial.md) — `AMORLINC`, `AMORLINCMTH`
- [System](functions/system.md) — `EVAL`, `NEWID`, `JsonParse`, `ApiBase`, …
- [Misc / LHS helpers](functions/misc.md) — `LHS()`, `LHSVALUE()`, `tmpl_foreach_operand`
- [Row Collection](functions/row-collection.md) — `ADDROWFIRST`, `IsSummaryRow`, `GetCallContextRow`, …
- [Cell and Row Styling](functions/styling.md) — `SETROWSTYLE`, `SETCELLSTYLE`

## Workbook context

- [App Variables](workbook-context/appvariables.md) — `AppVariables.Factor`, `getValue()`, recalc after change
- [Var syntax](workbook-context/var-syntax.md) — `@Var[name]` notation and assignment rules
- [Event context](workbook-context/event-context.md) — `@Event.Data`, `@Event.Sender`, `@Event.Selection`
- [Filter references](workbook-context/filter-references.md) — `Filter(group, name).SelectedValue`

## Rules

- [Cell Validation](rules/cell-validation.md) — `ValidationError` / `ValidationWarning`, `Validate()`
- [Conditional Cell Read-Only](rules/conditional-readonly.md) — making cells read-only based on data state
- [Row Styling](rules/row-styling.md) — CSS classes on rows and cells from Eaze conditions

## Extensibility

- [JavaScript Code-Behind](extensibility/js-code-behind.md) — inline JS in Worksheets/Tables (2025.5+)
- [Calculation Instance Factory](extensibility/calculation-instance-factory.md) — custom JS calc services via `<ComputeInstanceFactory>`

## Patterns

[Idiomatic patterns](patterns.md) — derived columns, cross-sheet lookups, dynamic `EVAL` formulas, AppVariable recalc, and more.
