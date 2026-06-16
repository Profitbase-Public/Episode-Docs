# Cell Validation

Cell validation rules flag cells that fail a condition, preventing saves or warning the user. Each rule has a **Name**, an optional **Disabled** flag, a **Target** expression, and a **Statement** expression.

## Rule structure

| Field | Description |
|---|---|
| **Target** | Cell-address Eaze expression — which cells to flag when Statement returns `false` |
| **Statement** | Boolean Eaze expression — returns `true` if the cell is valid, `false` if it should be flagged |

## Rule types

| Type | Behavior |
|---|---|
| `ValidationError` | Red border. Blocks save when `Validate()` runs from a Workbook action |
| `ValidationWarning` | Yellow border. Does not block save |

## Running validation

Call `Validate(terminateOnValidationFailed)` in a `SaveData` action to trigger the rules:

```
Validate(true)    -- stop the action chain if validation fails
Validate(false)   -- report errors but continue
```

`terminateOnValidationFailed` accepts a boolean or an Eaze expression.

## Important

`ValidationError` does **not** automatically prevent subsequent actions — it only blocks the save for that specific Worksheet. To gate an entire flow on validation across multiple Worksheets, build an Action Group that calls `Validate` on each Worksheet first, before any save actions.

## Example

```
Target:    @Qty[]
Statement: @Qty[] > 0
-- Flags Qty cells where the value is zero or negative
```
