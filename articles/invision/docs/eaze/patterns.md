# Eaze Patterns

Common idioms for Eaze cell calculations and Workbook conditions.

## Derived column

```
@Amount[] = Price * Qty;
```

## Row slice — formula on a subset of rows

```
@Amount[ProductID == "XP-5000"] =
    @Price[ProductID == "XP-5000"] * @Qty[ProductID == "XP-5000"];
```

## Cross-sheet lookup

```
@Amount[] = @Qty[] * @Price list!Price[ProductID == LHS().ProductID];
```

## Using a Workbook variable

```
@Amount[] = @TotalForce[] * AppVariables.Factor;
```

## Conditional read-only — lock cells after save

```
Target:    @Qty[]
Statement: @PBRowIdentity[] != -1
```

## Style the summary row

```
SETROWSTYLE(IsSummaryRow(GetCallContextRow()), "summary-css");
```

## Add and style a custom row

```
ADDROWFIRST({"ObjectId": -1, "AccountID": "F100", "ProductID": "P-001", "Total": 0, "P01": -3000});
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

## Dynamic formula via EVAL + template

Store a formula string in the data (e.g. `Formula = "L01 + L02"`), then evaluate it at runtime:

```
@Total[LineID == "L03"] = EVAL(tmpl_foreach_operand(Formula, "#LHSCOL[LineID == \"#OP\"]"));
-- Expands to: @Total[LineID == "L03"] = @Total[LineID == "L01"] + @Total[LineID == "L02"];
```

## Recalc after a Form Element changes an AppVariable

Add a `RecalcAction` call to the Form Element's `SelectionChanged` event handler:

```
RecalcAction();
```

See [Execution Model](language/execution-model.md) for why this is necessary.

## Workbook instructions accepting Eaze conditions

```
EnableIf(Filter("Filters", "Department").SelectedValue.Level >= 2 && _accountId == "A3000");
ExecuteIf(IsHostPageActive() && HasFilterChanged());
ToggleCssClass("highlight", X == 250);
ShowToastNotification("Update", CONCAT(_count, " items were updated"));
OpenBrowserWindow(CONCAT("http://mysite/api/invoices?id=", SelectedOrder.OrderId));
```
