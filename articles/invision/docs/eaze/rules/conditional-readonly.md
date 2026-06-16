# Conditional Cell Read-Only

Conditional read-only rules make specific cells non-editable based on row data or Workbook state. Each rule has a **Name**, an optional **Disabled** flag, a **Target** expression, and a **Statement** expression.

## Rule structure

| Field | Description |
|---|---|
| **Target** | Cell-address expression — which cells to make read-only when Statement returns `true` |
| **Statement** | Boolean Eaze expression — cells are read-only while this returns `true` |

## Examples

Make `Qty` read-only on rows where `Amount` exceeds a threshold:

```
Target:    @Qty[]
Statement: @Amount[] > 100
```

Make `Qty` read-only on rows already persisted to the database (`PBRowIdentity` is positive for saved rows):

```
Target:    @Qty[]
Statement: @PBRowIdentity[] != -1
```

Lock all cells on locked rows:

```
Target:    @Amount[]
Statement: IsRowLocked(GetCallContextRow())
```
