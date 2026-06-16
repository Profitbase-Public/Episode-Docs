# Eaze Calculations

Tables support cell calculations written in **Eaze** — Profitbase InVision's formula language for data grids. The syntax and function set are identical to Worksheets.

A calculation formula has this shape:

```
@<column>[<row-filter>] = <expression>;
```

```
@Amount[] = Price * Qty;
@Amount[ProductID == "XP-5000"] = @Price[ProductID == "XP-5000"] * @Qty[ProductID == "XP-5000"];
```

Multiple statements run in order. Each formula targets one or more cells by column name and an optional row predicate.

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

## Extensibility

- [JavaScript Code-Behind](../../eaze/extensibility/js-code-behind.md) — inline JS in Tables (2025.5+)
- [Calculation Instance Factory](../../eaze/extensibility/calculation-instance-factory.md) — custom JS calc services
