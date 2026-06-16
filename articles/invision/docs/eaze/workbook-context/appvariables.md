# App Variables in Formulas

Workbook variables are accessible in Eaze through `AppVariables`.

## Reading a variable

```
AppVariables.Factor                      -- dot-notation for simple names
AppVariables.getValue("Factor")          -- function form, equivalent
```

Use `getValue("…")` for variable names that contain spaces or use the `@Var[…]` syntax, since dot-notation cannot reach those:

```
AppVariables.getValue("@Var[My Variable]");
```

## Writing a variable

Writing `AppVariables` from within a cell formula is not supported. To set a variable from a Workbook action, use an Execute Expression action with the `@Var[…]` syntax. See [Var syntax](var-syntax.md).

## Recalculation after a variable change

If a grid formula references an `AppVariable` and that variable is updated from outside the grid — for example, from a Form Element's `SelectionChanged` event — the grid does **not** automatically re-run. You must trigger the Worksheet or Table's `RecalcAction` explicitly:

```
-- In the SelectionChanged action list:
RecalcAction();
```

See [Execution Model](../language/execution-model.md) for more on recalculation triggers.
