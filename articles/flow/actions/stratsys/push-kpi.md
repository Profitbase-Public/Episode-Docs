# Push data to Stratsys KPI API

The `Push data to Stratsys KPI API` action sends KPI data to the [Stratsys](https://www.stratsys.com/) KPI API. Use it to push measurement values from Flow into Stratsys — for example, to keep KPIs current from a scheduled sync instead of entering values manually.

The action works in two modes:

- **Entities** — push one or more prebuilt KPI entities. Build them first with [Create Stratsys KPI](./create-kpi.md) and pass them (or a list of them) to the **KPIs** input.
- **Tabular** — push directly from a [DataTable](../sql-server/load-to-datatable.md), without a separate build step. You supply the same mapping properties as [Create Stratsys KPI](./create-kpi.md) (KPI ID, Source, Department ID column, Period date column) on the action itself.

![Push data to Stratsys KPI API action showing Connection, Mode, KPIs, External source, and Version properties, with Success, Error, and Continue exit ports](/images/flow/stratsys-push-kpi.png)

<br/>

## When to use this

- Push KPI values calculated or stored in your data platform into Stratsys.
- Automate a nightly or weekly update of KPIs so Stratsys plans and reports stay current without manual entry.
- Load KPI data into Stratsys as the final step of a larger integration pipeline.

<br/>

## How it works

- **Input**: The KPI data to push (either prebuilt entities in **Entities** mode, or a source table with column mappings in **Tabular** mode) and an authenticated [Connection](./connection.md) to Stratsys.
- **Processing**: Flow authenticates against the Stratsys KPI API using OAuth client credentials and sends the KPI data. You do not need to manage tokens.
- **Output**: The API responses, returned on the **responses** output port. The action routes execution to the **Success**, **Error**, or **Continue** exit port depending on the result.

### Exit ports

| Port | Fires when |
|---|---|
| **Success** | The API returned a successful response. |
| **Error** | The API returned an error, or an exception occurred during execution. |
| **Continue** | Execution continues regardless of the outcome. |

<br/>

## Prerequisites

- A Stratsys subscription with access to the KPI API.
- A **Client ID** and **Client Secret** for your Stratsys account.
- A configured [Stratsys connection](./connection.md) in Flow. See [Create Stratsys connection](./create-connection.md) for how to set one up dynamically.

<br/>

## Properties

<!--prettier-ignore-->
| Name | Required | Description |
|---|---|---|
| **Connection** | Yes | The [connection](./connection.md) used to authenticate requests to the Stratsys KPI API. |
| **Dynamic connection** | No | When **Enable dynamic connection** is on, overrides the static **Connection** with a connection built at runtime by [Create Stratsys connection](./create-connection.md). |
| **Mode** | Yes | `Entities` to push prebuilt KPI entities, or `Tabular` to push directly from a source table. See the mode descriptions above. |
| **KPIs** | Entities mode | The KPI entity or list of entities to push. Typically the output of [Create Stratsys KPI](./create-kpi.md). |
| **KPI ID** | Tabular mode | The identifier of the Stratsys KPI the data belongs to. |
| **Source** | Tabular mode | The [DataTable](../sql-server/load-to-datatable.md) containing the rows to push. |
| **Department ID column** | No | (Tabular mode) The name of the source column that holds the department ID. |
| **Period date column** | No | (Tabular mode) The name of the source column that holds the period date. |
| **External source** | No | The source system, as defined in Stratsys, to tag pushed data with. Type the value directly into this field. Leave empty to use the default set in the connection. |
| **Version** | No | The Stratsys version to write to: `Active` or `Planning`. Defaults to `Active`. |
| **Responses variable name** | No | The name used to reference the API responses (the **responses** output). |
| **Description** | No | Free-text notes about the action. |

<br/>

## Returns

The API responses, returned on the **responses** output port as a `string` containing the raw response body from the Stratsys KPI API. Use this to inspect the outcome of the push — for example, to log which KPIs were accepted or to branch on errors.

A successful push (status 200) can still return warnings, for example about unrecognized columns:

```json
{
  "warnings": [
    {
      "nodeIdentifierId": 0,
      "message": "string",
      "departmentsWithWarnings": [
        {
          "departmentId": 0,
          "message": "string",
          "periodicDataWithWarnings": [
            {
              "periodDate": "string",
              "message": "string",
              "columnsWithWarnings": [
                {
                  "columnName": "string",
                  "message": "string"
                }
              ],
              "externalLinkWarning": {
                "url": "string",
                "externalSourceName": "string",
                "message": "string"
              }
            }
          ]
        }
      ]
    }
  ],
  "type": "string",
  "title": "string",
  "status": 0,
  "detail": "string",
  "instance": "string"
}
```

<br/>

## Example

The screenshot above shows a Flow that:

1. Declares a `stratsysKpis` list variable.
2. Reads a KPI Excel file from blob storage and [loads it into a DataTable](../sql-server/load-to-datatable.md).
3. Uses [Create Stratsys KPI](./create-kpi.md) to turn the table into a KPI entity (`kpi`).
4. Adds the entity to the `stratsysKpis` list.
5. Pushes the list to Stratsys with **Push data to Stratsys KPI API** in **Entities** mode.

<br/>

## See also

- [Create Stratsys KPI](./create-kpi.md) — build KPI entities from a `DataTable` or `DataReader`.
- [Stratsys connection](./connection.md) — set up a static connection to Stratsys.
- [Create Stratsys connection](./create-connection.md) — create a connection dynamically at runtime.
- [Load to DataTable](../sql-server/load-to-datatable.md) — run a SQL query and return the result as a `DataTable`.
