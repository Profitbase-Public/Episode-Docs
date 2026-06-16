# Row Collection Functions

These functions mutate the grid's bound data set in place, adding rows or inspecting row state.

## Adding rows

```
ADDROWFIRST(jsonObject)                    -- inserts row at the top of the grid
ADDROWLAST(jsonObject)                     -- appends row at the bottom
ADDROWAFTER(addressExpr, jsonObject)       -- inserts after the first row matching addressExpr
ADDROWBEFORE(addressExpr, jsonObject)      -- inserts before the first row matching addressExpr
```

Custom rows should include an `ObjectId` (commonly `-1`) so they can be targeted by styling, validation, or read-only rules:

```
ADDROWFIRST({"ObjectId": -1, "AccountID": "F100", "ProductID": "P-001", "Total": 0, "P01": -3000});
```

To style the custom row after adding it:

```
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

## Inspecting row state

```
IsSummaryRow(row)          -- true if the row is the summary (totals) row
IsRowLocked(row)           -- true if the row is locked for editing
GetCallContextRow()        -- returns the row that Eaze is currently evaluating
```

Typical use — style the summary row:

```
SETROWSTYLE(IsSummaryRow(GetCallContextRow()), "summary-css");
```
