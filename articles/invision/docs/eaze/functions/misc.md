# Misc Functions — LHS Helpers and AST Templating

## LHS

Returns the row object on the left-hand side of the current `=` assignment.

```
LHS()           -- the LHS row object
LHS().<prop>    -- access a property of that row
```

Used primarily in cross-spreadsheet addressing to correlate the current LHS row with rows on another sheet:

```
@Amount[] = @Qty[] * @Price list!Price[ProductID == LHS().ProductID];
```

## LHSVALUE

Returns the value of a named property on the LHS row. Use this when the property name is dynamic.

```
LHSVALUE("AccountID")    -- same as LHS().AccountID
```

## tmpl_foreach_operand

Builds a composite expression by substituting tokens from a space-separated `formula` string into an `operandTemplate`, then feeds the result to `EVAL`. Used for data-driven aggregation patterns.

```
tmpl_foreach_operand(formula, operandTemplate)
```

Substitution tokens available in `operandTemplate`:

| Token | Replaced with |
|---|---|
| `#LHSCOL` | Name of the LHS column |
| `#OP` | Current operand token from `formula` |

### Example

```
-- Formula column value: "L01 + L02"
-- LHS column: "Total"

@Total[LineID == "L03"] = EVAL(tmpl_foreach_operand(Formula, "#LHSCOL[LineID == \"#OP\"]"));

-- Expands to:
-- @Total[LineID == "L03"] = @Total[LineID == "L01"] + @Total[LineID == "L02"];
```
