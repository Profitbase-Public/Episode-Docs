# Stratsys overview

Flow includes built-in support for [Stratsys](https://www.stratsys.com/) through its KPI API, letting you push measurement data from a database, data platform, Excel file, or any other source supported by Flow into Stratsys — for example, reading KPI values from a SQL Server table or an Excel workbook and pushing them into Stratsys so plans and reports stay current without manual entry.

To use the Stratsys actions, you first need a [Stratsys connection](./connection.md) configured with a Tenant ID, Company code, and OAuth client credentials (Client ID and Client Secret). The same connection is reused across actions, or you can build one dynamically with [Create Stratsys connection](./create-connection.md) when credentials live outside Flow. Flow handles the OAuth token exchange automatically; you do not need to manage access tokens.

<br/>

## Explore

#### Setting up the connection
Set up a static [Stratsys connection](./connection.md) using a Tenant ID, Company code, Client ID, Client Secret, and an optional default external source. Or use [Create Stratsys connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Stratsys tenants from the same flow.

<br/>

#### Building KPI data
[Create Stratsys KPI](./create-kpi.md) turns a [DataTable](../sql-server/load-to-datatable.md) or [DataReader](../sql-server/get-datareader.md) into a Stratsys KPI entity, mapping your source columns (department, period) onto the fields the KPI API expects. Build one entity per KPI and collect them into a list to push several at once.

<br/>

#### Pushing data to Stratsys
[Push data to Stratsys KPI API](./push-data-to-kpi-api.md) sends KPI data to the Stratsys KPI API. It works in two modes: **Entities** pushes prebuilt entities from [Create Stratsys KPI](./create-kpi.md), and **Tabular** pushes directly from a [DataTable](../sql-server/load-to-datatable.md) using the same column mappings — no separate build step required.
