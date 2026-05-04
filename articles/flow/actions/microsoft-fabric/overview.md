# Microsoft Fabric overview

Flow includes built-in support for the [Microsoft Fabric REST API](https://learn.microsoft.com/en-us/rest/api/fabric/articles/), enabling automation of Fabric provisioning and operations. Common use cases include automatically provisioning Workspaces and Lakehouses, deploying Data Pipelines from version control (such as GitHub), uploading files into a Lakehouse for analytics, triggering pipeline runs, and refreshing semantic models.

To use any Fabric action, you first need a [Microsoft Fabric connection](./microsoft-fabric-connection.md) authenticated with a Microsoft Entra ID App (Service Principal) granted the appropriate **Power BI Service** application permissions (typically `Tenant.Read.All` and `Tenant.ReadWrite.All`). The connection page covers the full setup, including how to create the Entra ID App and grant admin consent.

> [!IMPORTANT]
> [Run Data Pipeline](./run-data-pipeline.md) is the one exception to the Service Principal pattern — the underlying Fabric REST API requires **user-delegated** permissions and does not support Service Principal authentication.

<br/>

## Explore

#### Provisioning Workspaces and Lakehouses
[Create Workspace](./create-workspace.md) provisions a Fabric Workspace, optionally attached to a specific Capacity. [Create Lakehouse](./create-lakehouse.md) creates a Lakehouse inside an existing Workspace, with options to enable schemas and to wait for full provisioning before continuing — important because the SQL Endpoint may not be ready immediately after creation.

<br/>

#### Loading data into a Lakehouse
[Upload to Lakehouse](./upload-to-lakehouse.md) uploads a file (byte array or stream) to a target folder in a Lakehouse, with overwrite control. [Load Lakehouse Table](./load-lakehouse-table.md) takes that further by loading the contents of a file or folder directly into a Lakehouse Table — supports load modes (append vs overwrite), source path types (file or folder), and source formats such as CSV.

<br/>

#### Working with Data Pipelines
[Create Data Pipeline](./create-data-pipeline.md) creates a Data Pipeline in a Workspace, optionally with a JSON definition (you can copy this from the Fabric UI via **View → View JSON code**, or pass it in Base64). This is useful for deploying pre-configured pipelines as part of standard data platform workloads. [Run Data Pipeline](./run-data-pipeline.md) executes a pipeline on demand and waits for it to complete, returning a `DataPipelineRunCompleted` object you can use to look up any output through additional actions.

<br/>

#### Refreshing semantic models
[Refresh semantic model](./refresh-semantic-model.md) triggers a refresh of a semantic model in a Workspace through the Power BI asynchronous refresh API. The action can be fire-and-forget or wait for completion. Note that only models using `Import` storage mode can be refreshed this way.

<br/>

#### Calling the Fabric REST API directly
[REST API request](./rest-api-request.md) calls any [Microsoft Fabric REST API](https://learn.microsoft.com/en-us/rest/api/fabric/articles/using-fabric-apis) endpoint. Use this whenever the operation you need isn't covered by one of the dedicated actions above — for example, deleting items, listing capacities, or working with Notebooks and Dataflows.

<br/>

[!INCLUDE [](./__videos.md)]


