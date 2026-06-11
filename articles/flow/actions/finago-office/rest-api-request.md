# REST API Request

The `REST API Request` action calls the [Finago Office (24SevenOffice) REST API](https://rest-api.developer.24sevenoffice.com/doc/v1/) and retrieves data. Use this action to pull data from Finago Office into a data platform, automate periodic data syncs, and build reporting pipelines over accounting, invoicing, and contact data.

![Finago Office REST API Request action showing Connection, Configuration, and exit ports for Success, Error, and Continue](/images/flow/24SevenOffice-rest-api.png)

**Example** ![Example](../../../../images/strz.jpg)  
The example above shows a Flow that uses a [dynamic connection](./create-connection.md) to request accounts. The JSON data in the response is [converted to a table](../json/get-json-datatable.md), and the resulting rows are [inserted into](../sql-server/insert-data.md) a SQL Server table.

<br/>

## When to use this

- Pull chart-of-accounts, general ledger transactions, invoices, or customer/vendor data from Finago Office into a SQL Server database or data warehouse.
- Automate a nightly or weekly sync of financial data so reporting dashboards stay current without manual exports.
- Extract data from Finago Office as part of a larger data integration pipeline that combines multiple source systems.

## How it works

- **Input**: A configured HTTP request (method, URI, optional query parameters) and an authenticated [Connection](./connection.md) to the Finago Office REST API.
- **Processing**: Flow authenticates against the Finago Office API using OAuth2 credentials (ClientId, ClientSecret, OrganizationId), sends the HTTP request, and handles retry logic for API throttling automatically. You do not need to manage tokens or retry cycles.
- **Output**: The response from the API, either as raw JSON (`HttpResponse<string>`) or a custom data type you define. The action routes execution to the **Success**, **Error**, or **Continue** exit port depending on the result.

### Exit ports

| Port | Fires when |
|---|---|
| **Success** | The API returned a successful response (HTTP 2xx). |
| **Error** | The API returned an error, or an exception occurred during execution. |
| **Continue** | Execution continues regardless of the outcome. |

<!-- TODO: Verify exact behavior of Continue port — does it fire in addition to Success/Error, or only when neither fires? -->

<br/>

## Prerequisites

- A Finago Office (24SevenOffice) subscription.
- A **ClientId**, **ClientSecret**, and **OrganizationId** for your Finago Office account.
- A configured [Finago Office Connection](./connection.md) in Flow. See [Create Finago Office Connection](./create-connection.md) for how to set one up dynamically.

<br/>

## Properties

<!--prettier-ignore-->
| Name | Required | Description |
|---|---|---|
| **Connection** | Yes | The [connection](./connection.md) used to authenticate requests to the Finago Office REST API. |
| **Dynamic connection** | No | Overrides the static **Connection** with credentials stored outside the Workspace (for example, in your own database). To create a connection dynamically at runtime, use the [Create Finago Office Connection](./create-connection.md) action. |
| **Configuration** | Yes | Defines the HTTP request to make to the API. See [Configuration](#configuration) below. |
| **Disabled** | No | When set to `true`, the action is skipped during Flow execution. |
| **Description** | No | Free-text notes about the action. |

<br/>

## Returns

The return type is defined when configuring the action. It can be a custom data type or the raw JSON response from the API.

Use the built-in [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) type to get additional information about the response, including the HTTP status code and any errors.

> [!TIP]
> For large data sets (10,000–200,000+ records), store the raw JSON response in a database or blob storage first, then use transformation tools (dbt, SQLMesh, or Azure Data Factory) to reshape the data. This avoids high memory usage from deserializing large payloads in Flow.

If you know the API returns a small number of records, you can use [Get JSON DataReader](../json/get-json-datareader.md) to flatten the JSON to a tabular format and process the data as rows and columns — for example, by inserting directly into a SQL Server table.

<br/>

## Configuration

To define a request to the REST API, you can start from a template or define it manually.

### Using a template

1. Open the **Configuration** dialog for the action.
2. Click **New Request**.
3. Choose from the list of predefined request templates.

The template collection covers a subset of the available APIs. If you cannot find a template for the request you need, see the [Finago Office REST API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) and define the request manually.

### Defining a request manually

See the [Finago Office REST API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) for available endpoints and parameters.

1. Set the **Method** (`GET`, `PUT`, `POST`, `DELETE`, etc.). Most endpoints that fetch data require `GET`.
2. Set the **URI** — the API endpoint path, for example `companies/{id}`. If the endpoint requires parameters (such as a company ID), insert a Flow variable using the popup that appears when the URI editor gets focus.
3. Add optional **Query Parameters**. For example, when requesting transaction lines, add `dateFrom` and `dateTo` parameters with date values in ISO format.
4. Define the **Response** type. The default is [HttpResponse&lt;string&gt;](../../api-reference/built-in-types/http-response.md), which returns the raw JSON. You can change this to a custom type, but keep in mind that deserialization allocates more memory and impacts performance for large data sets such as dimensions and general ledger queries.

<br/>

## Notes

- Flow handles OAuth2 authentication and API throttling/retry automatically. You do not need to manage access tokens or implement backoff logic.
- For large data sets, avoid deserializing into custom types directly. Store raw JSON first and transform it downstream.
- The Finago Office REST API is versioned independently of Flow. Check the [API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) for the latest available endpoints and any breaking changes.

<br/>

## See also

- [Finago Office Connection](./connection.md) — set up a static connection to the Finago Office API.
- [Create Finago Office Connection](./create-connection.md) — create a connection dynamically at runtime using credentials from a database or variable.
- [Get JSON DataTable](../json/get-json-datatable.md) — convert a JSON string to a `DataTable` for tabular processing.
- [Get JSON DataReader](../json/get-json-datareader.md) — stream JSON as rows and columns without loading the full payload into memory.
- [Insert Data](../sql-server/insert-data.md) — insert rows into a SQL Server table.
- [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) — built-in type that wraps an API response with status code and error details.
