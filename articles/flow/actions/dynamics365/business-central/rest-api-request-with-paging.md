# REST API Request with paging

Use the `REST API Request with paging` action to call [Dynamics 365 Business Central v2 APIs](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) and retrieve large amounts of data. A typical use case is building data integrations that pull data from Dynamics 365 Business Central into a data platform.

> [!NOTE]
> This action also works for APIs that do not return paged responses, in which case the response contains only a single page.


![Dynamics365 Bc API Request With Paging](/images/flow/dynamics365-bc-api-request-with-paging.png)

**Example** ![Example](/images/strz.jpg)  
This flow pulls general ledger transactions for every company in a Business Central environment into a database — a nested-paging integration. **Get companies** calls the Business Central API at its `URI` and returns the companies one page at a time through the `For each companiesResultPage` port. [foreach company](../../built-in/foreach.md)<!-- TODO: confirm the For Each loop action filename/path --> iterates the companies in each page (`items`) and exposes the current `company`. For each company, **Get general ledger transactions** calls the API again and pages through the matching entries via `For each generalLedgerEntries`, and [Save GL transactions to db](../../postgresql/insert-data.md) writes each batch to the target database. Use this pattern when you need to fan out a paged request per parent record — here, one general ledger query per company — and stream the result straight into a data platform.

<br/>

## Properties

<!--prettier-ignore-->
| Name          | Required | Description                                                                                                                                                                         |
| ------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Connection    | Yes | The [Dynamics 365 Business Central connection](./dynamics365-business-central-api-v2-connection.md) used to make an authenticated request to the Dynamics 365 Business Central API. |
| Dynamic connection | No | Use this option if you need to create a connection using credentials stored outside the workspace, for example in your own Azure SQL or PostgreSQL database. When this property is defined, Flow uses the `Dynamic connection` at runtime and `Connection` only at design time. To dynamically create a connection, use the [Create Dynamics 365 Business Central connection](./create-api-connection.md) action. |
| Configuration | Yes | Defines the HTTP request to make to the API. See details [below](#configuration). |


<br/>

## Returns

The return type is defined when configuring the action. It can be a custom data type or the raw JSON response from the API.  
We recommend the built-in [HttpResponse&lt;T&gt;](../../../api-reference/built-in-types/http-response.md) type because it includes additional information about the response, such as the HTTP status code and error(s).

We also recommend dumping the raw response to a data store, then using data transformation tools to shape it into a usable format. If the API returns small amounts of data (10 000 - 200 000 records), consider the [Get JSON DataReader](../../json/get-json-datareader.md) action to flatten the JSON to a tabular format and process the data as rows and columns, for example by inserting directly into a SQL Server table.

<br/>

## Configuration

To define a request to the Business Central API, you can start from a template or define it manually.
If you press the `New Request` button in the Configuration dialog, you can choose from a set of predefined request templates.  
The Business Central API is large, so the template collection contains only a subset of the available APIs. If you cannot find a template for the request you want, see the [Dynamics 365 Business Central v2 API documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) and define the request manually.

![New Request dialog with predefined Business Central request templates](/images/flow/dynamics365-bc-new-request.png)

### Defining a request manually

To define a request manually, see the [Dynamics 365 Business Central v2 API documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) to learn which endpoint and parameters to use.

1. Define the `Method` (`GET`, `POST`, `PUT`, or `DELETE`). Most APIs for fetching data require the `GET` method.
2. Define the `URI`. This is the API endpoint, for example `companies(id)/accounts`. If the endpoint requires parameters (such as the company id above), insert a variable from Flow using the popup that appears when the URI editor gets focus.
3. Define the `Response`. The default response type is [HttpResponse&lt;string&gt;](../../../api-reference/built-in-types/http-response.md), so you get back the raw JSON response from the API. You can change the data type to a custom type, but this allocates more memory and is slower for large data sets such as dimensions and general ledger queries. We recommend dumping the raw response to a data store (such as a database or blob storage), then using tools such as dbt, SQLMesh, or Azure Data Factory to shape it into a usable format.

## Response paging

The `REST API Request with paging` action handles paging automatically, returning one page at a time until the Dynamics 365 Business Central API returns no more pages.

## Error handling

If the response from the Dynamics 365 Business Central REST API is set to [HttpResponse&lt;T&gt;](../../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` property. When `IsSuccess` is false, the response has an `ErrorContent` property that relays the error messages from the API call or from internally thrown exceptions.
For other response types and for severe errors, the action raises an error that could terminate the Flow unless either the `On Error` port is connected or it is wrapped in a [Try-Catch](../../built-in/try-catch.md) action.
The `On Error` handler is triggered for each page error, so you can handle errors individually and prevent Flow from raising an error that might terminate the running process.

## API limits

The Dynamics 365 Business Central API has documented [API limits](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/dynamics-current-limits).

The `REST API Request with paging` action handles the following limit automatically:

- `429 - Too Many Requests`: Flow attempts to handle this limit by retrying the request using a retry policy. If the retries fail, an error is raised. You can handle this error in the `On Error` execution port, for example by using the [Wait](../../built-in/wait.md) action to perform a manual retry.