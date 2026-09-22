# Create Stratsys KPI

The `Create Stratsys KPI` action turns a [DataTable](../sql-server/load-to-datatable.md) or [DataReader](../sql-server/get-datareader.md) into a Stratsys KPI entity, mapping your source columns onto the fields the Stratsys KPI API expects. Use it to build the KPI data you then send with [Push data to Stratsys KPI API](./push-kpi.md).

Build one entity per KPI. To push several KPIs in one call, create each entity and collect them into a list (for example with an **Add to list** action), then pass the list to [Push data to Stratsys KPI API](./push-kpi.md) in **Entities** mode.

> [!TIP]
> If your data is already in a single [DataTable](../sql-server/load-to-datatable.md) and you want to push it directly, you can skip this action and use [Push data to Stratsys KPI API](./push-kpi.md) in **Tabular** mode, which takes the same mapping properties.

![Create Stratsys KPI action showing KPI ID, Source, Department ID column, and Period date column properties](/images/flow/stratsys-create-kpi.png)

<br/>

## When to use this

- Convert rows returned by a SQL query or read from an Excel file into a Stratsys KPI entity before pushing it to Stratsys.
- Build several KPI entities and collect them into a list to push in a single call.

<br/>

## How it works

- **Input**: A [DataTable](../sql-server/load-to-datatable.md) or [DataReader](../sql-server/get-datareader.md), plus the KPI identifier and the columns that hold the department and period.
- **Processing**: The rows are turned into a KPI entity for the KPI named in **KPI ID**. Each row provides a data point for the department in the **Department ID column** and the period in the **Period date column**. Every other column in the source is sent to Stratsys as a value column, using the source column name — these names must match the value names already defined for the KPI in Stratsys.
- **Output**: A Stratsys KPI entity, returned on the **kpi** output port, ready to pass to [Push data to Stratsys KPI API](./push-kpi.md).

<br/>

## Properties

<!--prettier-ignore-->
| Name | Required | Description |
|---|---|---|
| **KPI ID** | Yes | The identifier of the Stratsys KPI the data belongs to (for example `revenue_forecast`). |
| **Source** | Yes | The [DataTable](../sql-server/load-to-datatable.md) or [DataReader](../sql-server/get-datareader.md) containing the rows to convert. |
| **Department ID column** | No | The name of the source column that holds the department ID. |
| **Period date column** | No | The name of the source column that holds the period date. |
| **KPI variable name** | No | The name used to reference the resulting KPI entity (the **kpi** output). |
| **Description** | No | Free-text notes about the action. |

There is no separate property for the KPI value. All remaining columns in the **Source** (those not used as the Department ID or Period date column) are sent to Stratsys as value columns, using their column names as-is. These names must match the value names already defined for the KPI in Stratsys.

<br/>

## Returns

A single Stratsys KPI entity, returned on the **kpi** output port. Pass it (or a list of such entities) to the **KPIs** input of [Push data to Stratsys KPI API](./push-kpi.md).

<br/>

## See also

- [Push data to Stratsys KPI API](./push-kpi.md) — send the KPI entities to Stratsys.
- [Stratsys connection](./connection.md) — set up a connection to Stratsys.
- [Load to DataTable](../sql-server/load-to-datatable.md) — run a SQL query and return the result as a `DataTable`.
- [Get DataReader](../sql-server/get-datareader.md) — stream a SQL query result as a `DataReader`.
