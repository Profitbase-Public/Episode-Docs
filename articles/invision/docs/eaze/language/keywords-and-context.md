# Keywords and Context

## Built-in keywords

| Keyword | Meaning |
|---|---|
| `true` / `false` | Boolean literals |
| `null` | Null reference |
| `this` | Current execution context |

## `this` properties

`this` gives formulas access to data beyond the current row:

| Property | Type | Description |
|---|---|---|
| `this.Rows` | `any[]` | All rows in the current spreadsheet |
| `this.DataSets` | `{[id: string]: any[]}` | All rows in every spreadsheet loaded in the Workbook, keyed by spreadsheet id |
| `this.Variables` | `{[id: string]: any}` | All Workbook variables |
| `this.LhsRow` | `object` | The row on the left-hand side of `=` currently being processed |
| `this.LhsColumn` | `string` | The column name on the left-hand side of `=` currently being processed |

`this` is also where [Calculation Instance Factory](../extensibility/calculation-instance-factory.md) services are mounted — for example `this.my.calculator.add(...)`.
