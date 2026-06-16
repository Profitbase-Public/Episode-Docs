# Execution Model

## Statement ordering

An Eaze calculation block is a sequence of assignment statements separated by `;`. Statements execute top-to-bottom in the order they are written. A later statement can read a value written by an earlier one in the same block.

```
@Subtotal[] = Price * Qty;
@Tax[]      = @Subtotal[] * TaxRate;
@Total[]    = @Subtotal[] + @Tax[];
```

## Recalculation triggers

Formulas run automatically when:

- The grid loads data.
- A user edits a cell (the block re-runs after each edit).
- A `RecalcAction` instruction is called explicitly.

## AppVariable changes do not auto-trigger recalc

If a formula references an `AppVariable` (e.g. `AppVariables.Factor`) and that variable is updated from outside the grid — for example from a Form Element's `SelectionChanged` event — the grid does **not** automatically re-run calculations. You must call the Worksheet or Table's `RecalcAction` explicitly from the event handler.

```
-- In the Form Element's SelectionChanged action list:
RecalcAction();   -- triggers the Worksheet/Table to re-evaluate all formulas
```

See [App Variables](../workbook-context/appvariables.md) for the full variable reference.
