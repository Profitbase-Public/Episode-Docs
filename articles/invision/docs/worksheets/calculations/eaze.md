# Eaze Calculations

Worksheets support cell calculations written in **Eaze** — Profitbase InVision's formula language for data grids.

A calculation formula has this shape:

```
@<column>[<row-filter>] = <expression>;
```

```
@Amount[] = Price * Qty;
```

The left-hand side addresses the cell(s) to write to; the right-hand side is the expression that produces the value. Multiple statements run in order, top to bottom.

Eaze is also used in Worksheet rules — Cell Validation, Conditional Cell Read-Only, and Row/Cell Styling all use Eaze expressions for their Target and Statement fields.

## Full Eaze reference

All language details, operators, functions, and Workbook context are documented in the shared Eaze reference:

- [Eaze overview](../../eaze/index.md) — where Eaze is used and how to navigate the docs
- [Cell Addressing](../../eaze/language/cell-addressing.md) — column references, row predicates, cross-sheet `!`
- [Operators](../../eaze/language/operators.md)
- [Keywords and Context](../../eaze/language/keywords-and-context.md) — `this`, `this.Rows`, `this.DataSets`
- [Execution Model](../../eaze/language/execution-model.md) — statement order, `RecalcAction`
- [Function Reference](../../eaze/functions/index.md) — all built-in functions
- [App Variables](../../eaze/workbook-context/appvariables.md) — `AppVariables.Factor`, `getValue()`
- [Patterns](../../eaze/patterns.md) — idiomatic examples

## Worksheet rules

- [Cell Validation](../../eaze/rules/cell-validation.md) — `ValidationError` / `ValidationWarning`
- [Conditional Cell Read-Only](../../eaze/rules/conditional-readonly.md)
- [Row Styling](../../eaze/rules/row-styling.md)

## Extensibility

- [JavaScript Code-Behind](../../eaze/extensibility/js-code-behind.md) — inline JS in Worksheets (2025.5+)
- [Calculation Instance Factory](../../eaze/extensibility/calculation-instance-factory.md) — custom JS calc services
