# JavaScript Code-Behind

From InVision **2025.5**, Worksheets, Tables, and SQL (tabular) Reports support JavaScript written directly in the spreadsheet designer — no separate JS Solution object required. This covers calculations, business logic, custom rows, and Workbook action handlers.

## When to use JS instead of Eaze

Use JS code-behind when:

- Logic is too complex to express cleanly in Eaze
- You need async calls (`await`) in action handlers
- You want to use external libraries already loaded in the Workbook

Eaze remains the canonical language for straightforward in-grid cell calculations.

## Eaze → JavaScript translation

| Concept | Eaze | JavaScript |
|---|---|---|
| Event payload | `@Event.Data.customerId` | `Event.Data.customerId` |
| Workbook variable (legacy) | `@Var[My Var]` | `this.appVariables["@Var[My Var]"]` |
| Workbook variable (new) | `AppVariables.Factor` | `this.app.variables.get("Factor")` |
| Variable creation | Created on first assignment | Must be declared in the Variables dialog first |
| Async actions | Synchronous call | `await Execute(...)` required |

```js
// Eaze action handler
SetParamValue("@param1", @Event.Data.customerId);
_sqlScriptResponse = Execute("ExecSqlScriptFromWorkbook");

// JavaScript equivalent
SetParamValue("@param1", Event.Data.customerId);
_sqlScriptResponse = await Execute("ExecSqlScriptFromWorkbook");
```

## Key differences from Eaze

- `@Event` becomes `Event` (no `@` prefix).
- `@Var[…]` variables must be read via `this.appVariables["@Var[…]"]`.
- Async actions (`WebApi`, `Execute Web Function`, `Execute SQL Script`) must be awaited.
- Workbook variables must be declared before use; they are not created on first assignment.

## Heavier alternative

For scenarios requiring a shared, stateful calculation service, see [Calculation Instance Factory](calculation-instance-factory.md).
