# Finago Office overview

Flow includes built-in support for [Finago Office (24SevenOffice)](https://rest-api.developer.24sevenoffice.com/doc/v1/) through its REST API, letting you pull data from Finago Office into a database, data platform, or any other destination supported by Flow — for example, requesting accounts and inserting them into a SQL Server table for downstream reporting.

To use the Finago Office action, you first need a [Finago Office connection](./connection.md) configured with a Client ID, Client Secret, and Organization ID. The same connection is reused across actions, or you can build one dynamically with [Create Finago Office connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Finago Office connection](./connection.md) using a Client ID, Client Secret, and Organization ID, or use [Create Finago Office connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters. The credentials are generated following [Finago Office's documentation for access tokens](https://rest-api.developer.24sevenoffice.com/doc/v1/topic/topic-get-access-token).

<br/>

#### Calling the Finago Office API
Two actions cover the Finago Office REST API, depending on whether the endpoint returns paginated data.  [REST API Request](./rest-api-request.md) calls any Finago Office endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as transaction lines. You can either pick from built-in request templates for common endpoints, or define the request manually (Method, URI, Query Parameters, Response type). For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards using tools like dbt, SQLMesh, or Azure Data Factory — using a custom response type instead of raw JSON allocates more memory and may impact performance for queries returning large amounts of data such as general ledger entries.

