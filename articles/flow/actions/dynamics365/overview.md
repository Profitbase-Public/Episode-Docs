# Dynamics 365 overview

Flow includes built-in support for [Dynamics 365 Business Central](https://learn.microsoft.com/en-us/dynamics365/business-central/) through its [v2 REST API](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/), letting you pull data from a Business Central environment into a database, data platform, or any other destination supported by Flow. The actions in this category currently target Business Central specifically — other Dynamics 365 products (such as Sales or Customer Service) are not covered by this category.

To use any Dynamics 365 action, you first need a [Business Central API v2 connection](./business-central/dynamics365-business-central-api-v2-connection.md) authenticated with a Microsoft Entra ID App (also known as a Service Principal) that has been granted access to the Business Central API. The same connection is reused across all actions, or you can build one dynamically with [Create Business Central API v2 connection](./business-central/create-api-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Business Central API v2 connection](./business-central/dynamics365-business-central-api-v2-connection.md) using a Microsoft Entra ID App, or use [Create Business Central API v2 connection](./business-central/create-api-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters. The connection page covers the full setup: creating the Entra ID App, granting it `API.ReadWrite.All` and `Automation.ReadWrite.All` permissions, getting admin consent, and registering it in the Business Central admin portal.

<br/>

#### Calling the Business Central API
[REST API Request](./business-central/rest-api-request.md) calls any Business Central v2 API endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`).
[REST API Request with paging](./business-central/rest-api-request-with-paging.md) calls any Business Central v2 API endpoint and handles paging automatically, returning one page at a time until the API has no more data. You can either pick from built-in request templates for common endpoints or define the request manually (Method, URI, Response type). 
The actions also handles `429 Too many requests` automatically using a retry policy, and exposes an **On Error** port for handling per-page errors without terminating the flow. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards using tools like dbt, SQLMesh, or Azure Data Factory.
