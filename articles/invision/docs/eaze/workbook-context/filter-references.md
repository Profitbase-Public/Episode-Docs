# Filter References

Filter selected values are accessible in Eaze through `Filter(group, name)`.

## Syntax

```
Filter("<group>", "<filterName>").SelectedValue
```

## SelectedValue properties

| Property | Type | Description |
|---|---|---|
| `Id` | `string` | Identifier of the selected filter item |
| `ColumnName` | `string` | Column name the filter is bound to |
| `Description` | `string` | Display text of the selected item |
| `IsLeaf` | `boolean` | `true` if the selected item is a leaf node (no children) |
| `Level` | `number` | Hierarchy level of the selected item |

## Examples

```
-- Pass selected department ID as a parameter
SetParamValue("@MyParam", Filter("MainFilters", "Department").SelectedValue.Id);

-- Store selected name in a variable
_departmentName = Filter("Page1Filters", "Department").SelectedValue.Description;

-- Enable a button only when a leaf-level department is selected
EnableIf(Filter("Filters", "Department").SelectedValue.IsLeaf);

-- Combine filter level with another condition
EnableIf(Filter("Filters", "Department").SelectedValue.Level >= 2 && _accountId == "A3000");
```
