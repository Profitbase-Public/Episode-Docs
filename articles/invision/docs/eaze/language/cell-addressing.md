# Cell Addressing

Eaze targets cells by column name plus an optional row-selector predicate.

## Syntax

```
@<column>[]                  -- all cells in <column>
@<column>[<predicate>]       -- cells in <column> for rows matching <predicate>
```

The predicate runs per row against the grid's data set and must evaluate to `true` or `false`. It may reference column values from the current row, Eaze functions, and logical operators.

```
@Amount[] = 10;
@Amount[AccountID == "N-3000"] = 10;
@Amount[AccountID == "2000" && ProjectID == "N-500"] = 10;
@Amount[AccountID == "2000" && LEFT(ProjectID, 1) == "N"] = 10;
```

## Cross-spreadsheet addressing

Reference a column in another spreadsheet in the same Workbook using the `!` separator. Use `LHS()` to correlate the current LHS row with a row on the other sheet.

```
@Amount[] = @Qty[] * @Price list!Price[ProductID == LHS().ProductID];
```

Reads `Price` from the spreadsheet named `Price list`, matched on `ProductID` to the current row being calculated.

## Time Frame column date metadata

For Time Frame columns (`P01`, `P02`, …), read the column's underlying date via the `@Property[]` expression:

```
@Property[P03.Date]      -- the date represented by column P03
```

This is the recommended way to work with period dates. See [Time Frame column helpers](../functions/timeframe-dates.md) for caption-building functions.

## Custom-row addressing

Rows added via `ADDROW*` functions need an `ObjectId` (commonly `-1`) to be addressable in styling, validation, or read-only rules:

```
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

See [Row Collection functions](../functions/row-collection.md) for `ADDROWFIRST` and related functions.
