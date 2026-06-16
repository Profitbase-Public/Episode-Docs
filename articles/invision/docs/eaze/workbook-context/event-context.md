# Event Context

Inside a Workbook action's instruction list, `@Event` exposes the event payload that triggered the action.

## Common properties

```
@Event.Data.<Field>          -- field on the row that was clicked or acted on
@Event.Sender.ColumnName     -- column where the action originated
@Event.Sender.ActionName     -- name of the menu action or button
@Event.Selection.Rows        -- array of selected rows
@Event.Selection.Cells       -- selected cells
```

## Example

```
SetParamValue("@param1", @Event.Data.customerId);
@Var[Last Row Clicked] = @Event.Data;
@Var[Last Row Clicked].ProductID = @Event.Data.ProduktID;
```

## In JavaScript action handlers

In JavaScript action handlers (2025.5+), use `Event` without the `@` prefix:

```js
SetParamValue("@param1", Event.Data.customerId);
const rows = Event.Selection.Rows;
```

See [JavaScript Code-Behind](../extensibility/js-code-behind.md) for the full Eaze → JS mapping.
