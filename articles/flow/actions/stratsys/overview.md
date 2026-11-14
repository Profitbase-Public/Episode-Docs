# Stratsys overview

Flow includes built-in support for [Stratsys](https://www.stratsys.com/) through its KPI API and REST API. You can push measurement data from a database, data platform, Excel file, or any other source supported by Flow into Stratsys — for example, reading KPI values from a SQL Server table or an Excel workbook and pushing them into Stratsys so plans and reports stay current without manual entry. You can also read data out of Stratsys, such as KPI data, departments, users, scorecards, SCIM users, and due diligence data.

To use the Stratsys actions, you first need a [Stratsys connection](./connection.md) configured with a Company code and either OAuth client credentials (Tenant ID, Client ID, and Client Secret) or a SCIM key. The same connection is reused across actions, or you can build one dynamically with [Create Stratsys connection](./create-connection.md) when credentials live outside Flow. Flow handles the authentication for you; you do not need to manage access tokens.

<br/>

## Explore

#### Setting up the connection
Set up a static [Stratsys connection](./connection.md) using a Company code and an authentication mode: **OAuth client credentials** (Tenant ID, Client ID, and Client Secret) or **SCIM bearer token** (SCIM key), plus an optional default external source. Or use [Create Stratsys connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Stratsys tenants from the same flow.

<br/>

#### Building KPI data
[Create Stratsys KPI entity](./create-kpi-entity.md) turns a [DataTable](../sql-server/load-to-datatable.md) or [DataReader](../sql-server/get-datareader.md) into a Stratsys KPI entity, mapping your source columns (department, period) onto the fields the KPI API expects. Build one entity per KPI and collect them into a list to push several at once.

<br/>

#### Pushing data to Stratsys
[Push data to Stratsys KPI API](./push-data-to-kpi-api.md) sends KPI data to the Stratsys KPI API. It works in two modes: **Entities** pushes prebuilt entities from [Create Stratsys KPI entity](./create-kpi-entity.md), and **Tabular** pushes directly from a [DataTable](../sql-server/load-to-datatable.md) using the same column mappings — no separate build step required.


<br/>

#### Calling the Stratsys API
Two actions cover the Stratsys REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any Stratsys endpoint and returns a single response — for example KPI data, departments, users, scorecards, nodes, or export views. [REST API Request with paging](./paged-rest-api-request.md) handles paginated collections such as SCIM users and groups or due diligence data, running once per page with configurable **Paging mode** (Page or SCIM), **Items per page** (default 50), and **Max page count** (default 1000). Both actions support starting from built-in request templates or defining requests manually, and both handle `429 Too Many Requests` rate limits automatically by delaying and retrying. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.
