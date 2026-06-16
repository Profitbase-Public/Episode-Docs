# Row Styling

Row and cell styling is defined as Eaze statements that run on data load and after each recalculation. The statements call `SETROWSTYLE` or `SETCELLSTYLE` to apply CSS classes.

## SETROWSTYLE

```
SETROWSTYLE(rowAddressExpr, cssClass: string)
```

```
SETROWSTYLE(ProductID == "XYZ", "ProductGroup1");
SETROWSTYLE(STARTSWITH(ProductID, "F"), "StyleA");
```

## SETCELLSTYLE

```
SETCELLSTYLE(rowAddressExpr, columnName: string, cssClass: string)
```

```
SETCELLSTYLE(Total < 1000, "Total", "RedCell");
```

## Special rows

Style the summary row:

```
SETROWSTYLE(IsSummaryRow(GetCallContextRow()), "SummaryRow");
```

Style a custom row added via `ADDROWFIRST` (identified by `ObjectId == -1`):

```
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

## Notes

- The CSS classes must be defined in your solution's stylesheet.
- Multiple rules can apply different classes to the same row.
- See [Cell and Row Styling functions](../functions/styling.md) for the full function signatures.
- See [Row Collection functions](../functions/row-collection.md) for `ADDROWFIRST`, `IsSummaryRow`, and `GetCallContextRow`.
