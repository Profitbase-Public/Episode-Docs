# REST API Request

Use the `REST API Request` action to call [Dynamics 365 Business Central v2 APIs](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) and read or write data with HTTP methods such as `GET`, `POST`, `PUT`, and `DELETE`. Typical use cases include creating, updating, or deleting records in Dynamics 365 Business Central, or retrieving smaller data sets that do not require paging.

> [!NOTE]
> If you need to retrieve large amounts of data from a paged API endpoint, use the [REST API Request with paging](./rest-api-request-with-paging.md) action instead.


![Fetch latest currency rate flow branching on the Success and Error ports of the REST API Request action](/images/flow/dynamics365-bc-api-request.png)

**Example** ![Example](/images/strz.jpg)  
This flow retrieves the latest currency rate from Business Central and branches on the outcome instead of letting a failed call terminate the run. **Fetch latest currency rate** is the `REST API Request` action: it calls the Business Central API at the given `URI` and exposes the response on its `CurrencyRate` output. On `Success`, [Process currency rate](../../built-in/function.md) receives the response as `item` and processes it for downstream use. On `Error`, [Log error: Failed to retrieve data](../../built-in/log.md) records the failure from its `data` input. Use this pattern when a failed API call should be handled in the graph itself — branching on the `Success` and `Error` ports — rather than wrapped in a [Try-Catch](../../built-in/try-catch.md) scope.

<br/>


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

For `GET` requests that return data, we also recommend dumping the raw response to a data store (such as a database or blob storage), then using data transformation tools to shape it into a usable format. If the response is small, use the [Get JSON DataReader](../../json/get-json-datareader.md) action to flatten the JSON to a tabular format and process the data as rows and columns, for example by inserting directly into a SQL Server table.

<br/>

## Configuration

To define a request to the Business Central API, you can start from a template or define it manually.
If you press the `New Request` button in the Configuration dialog, you can choose from a set of predefined request templates.  
The Business Central API is large, so the template collection contains only a subset of the available APIs. If you cannot find a template for the request you want, see the [Dynamics 365 Business Central v2 API documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) and define the request manually.

![New Request dialog with predefined Business Central request templates](/images/flow/dynamics365-bc-new-request.png)

### Defining a request manually

To define a request manually, see the [Dynamics 365 Business Central v2 API documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/) to learn which endpoint and parameters to use.

1. Define the `Method` (`GET`, `POST`, `PUT`, `PATCH`, or `DELETE`).
   - `GET`: Retrieve data from Business Central (for example, a single record by id).
   - `POST`: Create new records (for example, a new customer or sales invoice).
   - `PUT` / `PATCH`: Update existing records.
   - `DELETE`: Remove records.
2. Define the `URI`. This is the API endpoint, for example `companies(id)/customers(id)`. If the endpoint requires parameters (such as the company id above), insert a variable from Flow using the popup that appears when the URI editor gets focus.
3. Define the `Body` (for `POST`, `PUT`, and `PATCH` requests). The body is the JSON payload sent to the API.
4. Define the `Response`. The default response type is [HttpResponse&lt;string&gt;](../../../api-reference/built-in-types/http-response.md), so you get back the raw JSON response from the API. You can change the data type to a custom type, but this allocates more memory and is slower for large data sets.

> [!NOTE]
> Authentication headers are automatically set up from the connection settings.

<br/>

## Error handling

If the response from the Dynamics 365 Business Central REST API is set to [HttpResponse&lt;T&gt;](../../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` property. When `IsSuccess` is false, the response has an `ErrorContent` property that relays the error messages from the API call or from internally thrown exceptions.
For other response types and for severe errors, the action raises an error that could terminate the Flow unless either the `On Error` port is connected or it is wrapped in a [Try-Catch](../../built-in/try-catch.md) action.

## API limits

The Dynamics 365 Business Central API has documented [API limits](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/dynamics-current-limits).

The `REST API Request` action handles the following limit automatically:

- `429 - Too Many Requests`: Flow attempts to handle this limit by retrying the request using a retry policy. If the retries fail, an error is raised. You can handle this error in the `On Error` execution port, for example by using the [Wait](../../built-in/wait.md) action to perform a manual retry.