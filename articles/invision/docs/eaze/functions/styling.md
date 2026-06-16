# Cell and Row Styling Functions

Apply CSS classes to rows or individual cells based on Eaze conditions. The CSS classes must be defined in your solution's stylesheet.

## SETROWSTYLE

```
SETROWSTYLE(rowAddressExpr, cssClass: string)
```

`rowAddressExpr` is a boolean predicate evaluated per row — rows where it returns `true` receive the class.

```
SETROWSTYLE(ProductID == "XYZ", "ProductGroup1");
SETROWSTYLE(STARTSWITH(ProductID, "F"), "StyleA");
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");        -- style a custom/added row
SETROWSTYLE(IsSummaryRow(GetCallContextRow()), "SummaryRow");
```

## SETCELLSTYLE

```
SETCELLSTYLE(rowAddressExpr, columnName: string, cssClass: string)
```

Applies the class only to the named column's cell on matching rows.

```
SETCELLSTYLE(Total < 1000, "Total", "RedCell");
```

## Notes

- Both functions run on data load and after each recalculation.
- Styling via these functions is additive — multiple rules can apply classes to the same row or cell.
- See [Row Styling rules](../rules/row-styling.md) for how these functions fit into the broader rule system.
