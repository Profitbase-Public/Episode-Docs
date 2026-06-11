# Finago Office REST API Request with paging

The `REST API Request with paging` action retrieves large, paginated datasets from the [Finago Office (24SevenOffice) REST API](https://rest-api.developer.24sevenoffice.com/doc/v1/). The action iterates through all pages returned by the API and fires once per page, so downstream actions process one page at a time without you needing to manage pagination logic.

![Flow example showing Paged REST API Request retrieving transaction lines, with Get JSON DataReader and Insert rows on the For each path, and Log error on the On Error path](/images/flow/finagooffice-rest-api-paging.png)

**Example** ![Example](/images/strz.jpg)

In the example above, a **Paged REST API Request** retrieves transaction lines from the Finago Office API. For each page, the JSON response is converted to rows using the [Get JSON DataReader](../json/get-json-datareader.md) action, and the rows are [inserted into](../sql-server/insert-data.md) a SQL Server table. If a page fails, the **On Error** port routes to a **Log error** action instead of terminating the Flow.

<br/>

## When to use this

- Retrieve transaction lines, general ledger entries, or other large datasets from Finago Office where the API returns results across multiple pages.
- Build a nightly data sync that pulls all records from Finago Office into a SQL Server staging table for downstream reporting.
- Process paginated API responses page-by-page to keep memory usage low, instead of loading the entire result set at once.

Use the non-paging [REST API Request](./rest-api-request.md) action when the endpoint returns a single response that does not need pagination.

## How it works

- **Input**: An HTTP request definition (method, URI, optional parameters) and an authenticated [Connection](./connection.md) to the Finago Office REST API.
- **Processing**: Flow authenticates using OAuth2 credentials, sends the request, and retrieves one page of results at a time. For each page, the action fires the **For each \<endpoint\>** exit port, passing the page data to downstream actions. Pagination continues until no more pages remain or the **Max page count** limit is reached. Flow handles API throttling (HTTP 429) automatically with retry logic.
- **Output**: Each page is returned as the configured response type — either raw JSON (`HttpResponse<string>`) or a custom data type. After all pages have been processed (or on error), the **Continue** port fires.

### Exit ports

| Port | Fires when |
|---|---|
| **For each \<endpoint\>** | Once per page of results. The port name reflects the URI endpoint (for example, "For each transactionlines"). Downstream actions connected here run for every page. |
| **On Error** | A page request failed after retries, or an exception occurred. Connected error-handling actions run per failed page without terminating the Flow. |
| **Continue** | After all pages have been processed (or the action completes with an error that is not handled by On Error). |

<br/>

## Prerequisites

- A Finago Office (24SevenOffice) subscription.
- A **ClientId**, **ClientSecret**, and **OrganizationId** for your Finago Office account.
- A configured [Finago Office Connection](./connection.md) in Flow. See [Create Finago Office Connection](./create-connection.md) for how to set one up dynamically.

<br/>

## Properties

| Name | Required | Description |
|---|---|---|
| **Title** | No | Display name for the action on the canvas. |
| **Connection** | Yes | The [Finago Office Connection](./connection.md) used to authenticate requests to the Finago Office REST API. |
| **Dynamic connection** | No | Overrides the static **Connection** with credentials created at runtime by the [Create Finago Office Connection](./create-connection.md) action. When set, Flow uses the dynamic connection at runtime and the static **Connection** only at design time. |
| **Configuration** | Yes | Defines the HTTP request to the API, including method, URI, parameters, and return type. See [Configuration](#configuration) below. |
| **Start page** | No | Offset to the starting page for data retrieval. Defaults to `1`. |
| **Items per page** | No | Number of items to retrieve per page. Defaults to `50`. |
| **Max page count** | No | Maximum number of pages to fetch. Defaults to `9999`. |
| **Disabled** | No | When set to `true`, the action is skipped during Flow execution. |
| **Description** | No | Free-text notes about the action. |

<br/>

## Returns

The return type is defined when configuring the action. It can be a custom data type set by the template, or the raw JSON response from the API.

Use the built-in [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) type to get additional information about the response, including the HTTP status code and any errors.

> [!TIP]
> For large datasets, store the raw JSON response in a database or blob storage first, then use transformation tools (dbt, SQLMesh, or Azure Data Factory) to reshape the data. This avoids high memory usage from deserializing large payloads in Flow.

If the API returns a relatively small number of records (10,000–200,000), you can use [Get JSON DataReader](../json/get-json-datareader.md) to flatten the JSON into a tabular format and process the data as rows and columns — for example, by inserting directly into a SQL Server table.

<br/>

## Configuration

To define a request to the Finago Office REST API, start from a template or define it manually.

### Using a template

1. Open the **Configuration** dialog for the action.
2. Click **New Request**.
3. Choose from the list of predefined request templates.

<!-- TODO: Replace screenshot — currently shows Dynamics 365 BC template picker, not Finago Office. -->
![New Request dialog showing predefined request templates](/images/flow/dynamics365-bc-new-request.png)

The template collection covers a subset of the available Finago Office APIs. If you cannot find a template for the request you need, see the [Finago Office REST API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) and define the request manually.

### Defining a request manually

See the [Finago Office REST API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) for available endpoints and parameters.

1. Set the **Method** (`GET`, `PUT`, `POST`, `DELETE`). Most endpoints that fetch data require `GET`.
2. Set the **URI** — the API endpoint path, for example `transactionlines`. If the endpoint requires parameters, insert a Flow variable using the popup that appears when the URI editor gets focus.
3. Add optional **Parameters** as query or body parameters required by the endpoint. Use variables or fixed values depending on your workflow.
4. Define the **Response** type. The default is [HttpResponse&lt;string&gt;](../../api-reference/built-in-types/http-response.md), which returns the raw JSON. You can change this to a custom type, but deserialization allocates more memory and impacts performance for large datasets. For large datasets, store raw JSON and transform it downstream.

> [!NOTE]
> Flow sets authentication headers automatically from the Connection. You do not need to configure auth headers manually.

<br/>

## Response paging

The action handles pagination automatically. It requests one page at a time and fires the **For each \<endpoint\>** exit port for each page, until the API returns no more pages or the **Max page count** is reached.

This action also works with endpoints that do not return paginated responses. In that case, the **For each** port fires once with the complete result.

<br/>

## Error handling

If the response type is set to [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` property. When `IsSuccess` is `false`, the `ErrorContent` property contains the error messages from the API call or from internally thrown exceptions.

For other response types and for severe errors, the action raises an error that terminates the Flow unless either the **On Error** port is connected or the action is wrapped in a [Try-Catch](../built-in/try-catch.md) action.

The **On Error** handler fires for each failed page individually, so you can log or retry page-level errors without terminating the entire Flow.

<br/>

## API limits

Finago Office enforces rate limits to maintain stable server performance. If you exceed these limits, the API returns a `429 Too Many Requests` error.

The action handles this automatically by delaying and retrying requests. If the retry limit is reached, an error is raised. You can handle this error in the **On Error** port — for example, by using the [Wait](../built-in/wait.md) action to pause before a manual retry.

<br/>

## See also

- [REST API Request](./rest-api-request.md) — non-paging version for endpoints that return a single response.
- [Finago Office Connection](./connection.md) — set up a static connection to the Finago Office API.
- [Create Finago Office Connection](./create-connection.md) — create a connection dynamically at runtime.
- [Get JSON DataReader](../json/get-json-datareader.md) — stream JSON as rows and columns without loading the full payload into memory.
- [Insert Data](../sql-server/insert-data.md) — insert rows into a SQL Server table.
- [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) — built-in type that wraps an API response with status code and error details.
- [Try-Catch](../built-in/try-catch.md) — wrap actions in error-handling logic.
