# Stratsys REST API Request with paging

Use the Stratsys REST API to retrieve paginated data.

The **REST API request with paging** action enables you to interact with the Stratsys REST API to retrieve large, paginated datasets. This action simplifies working with endpoints that return multiple pages of data, such as SCIM users and groups or due diligence suppliers, categories, groups, tags, and users. Pagination is managed automatically, allowing you to focus on processing the data.

The action always uses the `GET` method.

<br/>

## Properties

| Name            | Required | Description          |
|---------------- | -------- | ---------------------|
| Title           | No | The title or name of the request.  |
| Connection      | Yes | The [Stratsys Connection](./connection.md) used to make an authenticated request to the Stratsys REST API. |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Stratsys connection](./create-connection.md) action. |
| Configuration   | Yes | Specifies the HTTP request to the Stratsys API, including the URL, parameters, and return type. |
| Paging mode     | No | How the endpoint is paged. `Page (page/pageSize)` is the default. Use `SCIM (startIndex/count)` for SCIM endpoints. See [Paging modes](#paging-modes). |
| Start page      | No | Page mode only. The page number to start from. Defaults to `1`. |
| Start index     | No | SCIM mode only. The index to start from. Defaults to `1`. |
| Items per page  | No | The number of items to retrieve per page. Defaults to `50`. The maximum is `200` in Page mode. |
| Max page count  | No | The maximum number of pages to fetch. Defaults to `1000`. |
| Description     | No | Additional notes or comments about the action or configuration. |

<br>

## Paging modes

Stratsys uses two different paging styles, depending on the endpoint.

| Mode | Query parameters | Response fields | Used by |
|------|------------------|-----------------|---------|
| Page | `page`, `pageSize` | `items`, `totalCount` | Due diligence endpoints |
| SCIM | `startIndex`, `count` | `Resources`, `totalResults` | SCIM users and groups |

The paging query parameters are managed by the action. Do not add them yourself under **Parameters**.

When you select a template or enter a URI that contains `scim/`, the paging mode is switched to **SCIM** automatically. For any other URI, it is set to **Page**. You can change the mode manually if you define a request that doesn't follow this convention.

<br>

## Returns

The action runs the **For each page** branch once for every page that is returned, not once for every item. The current page is available as a variable (named `page` by default) that is only valid inside that branch.

The return type is defined when configuring the action. It can be a custom data type, or the raw JSON response from the API.
We recommend using the built-in [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) type because it will include additional information about the response, such as the HTTP status code and the content.

We recommend dumping the raw response into a data store and using data transformation tools to convert it into a usable format. If the API returns a relatively small dataset (10,000–200,000 records), consider using the [Get JSON DataReader](../json/get-json-datareader.md) to flatten the JSON into a tabular format. This allows you to process the data as rows and columns, such as by inserting it directly into a SQL Server table.

<br/>

## Execution outputs

| Port | Description |
|------|-------------|
| For each page | Runs once per successfully retrieved page. Connect the actions that process the page here. |
| Error | Runs if a page request fails. Paging stops, and the error details are available through the `apiException` variable (`StatusCode`, `ResponseContent`). |
| Continue | Runs once after all pages have been fetched, or after an error has been handled. The page variable is not available here. |

<br/>

## Configuration

To define a request to the Stratsys REST API, you can start from a template, or define it manually.
If you press the `New Request` button in the Configuration dialog, you can choose from a set of predefined request templates for paged endpoints:

| Group | Templates |
|-------|-----------|
| SCIM | Get SCIM users, Get SCIM groups |
| Due diligence | Get due diligence suppliers, categories, groups, tags, and users |

If you cannot find a template for the request you want to make, you can refer to the Stratsys API documentation for your tenant and define the request manually. Which endpoints you can call depends on the API permissions granted to your Stratsys tenant and credentials.

1. **URI**: Specify the collection endpoint for your request, relative to your Stratsys API address. For example:
   - `v1/scim/users`: To read SCIM users.
   - `v1/duediligence/suppliers`: To read due diligence suppliers.

2. **Headers**:
   - Authentication is automatically set up from the connection settings.

3. **Parameters**: Add additional query parameters as required by the endpoint, for example filters. Use variables or fixed values based on your workflow to customize the request.

4. **Response Type**: Use the default `HttpResponse<string>` to work with raw JSON data. For large datasets, this approach is recommended to reduce memory usage and improve performance.

> [!NOTE]
> SCIM endpoints require a connection that uses **SCIM bearer token** authentication. Other endpoints use **OAuth client credentials**. See [Connecting to Stratsys](./connection.md#authentication-modes).

<br/>

## Error handling

HTTP failures raise an error regardless of the response type. Paging stops and no failed page is passed to the **For each page** branch, so you do not need to check `IsSuccess` on the page. Handle failures by connecting the **Error** port, or by wrapping the action in a [Try-Catch](../built-in/try-catch.md) block. Without a handler the error is raised and could terminate the Flow.

Errors raised by actions inside the **For each page** branch are not handled by the **Error** port of this action.

<br>

## API Limits

Stratsys enforces rate limits to maintain stable server performance. If you exceed these limits, the API will return a `429 Too Many Requests` error.
The action handles this by delaying calls and retrying requests. If the retry limit is reached, an error is returned.

<br/>

## Workflow Example

1. **Configure the Action**:
   - Select a template such as **Get SCIM users**, or define the endpoint manually.
   - Set **Items per page** and **Max page count** as needed.

2. **Handle the Response**:
   - Connect the processing actions to **For each page**, and use the page variable as their input. For example, store the page's raw JSON in a database or file.
   - Connect the **Error** port to handle failures.

3. **Post-Processing**:
   - Connect actions to **Continue** to transform or analyze the stored data after all pages have been retrieved.

<br/>

By using the **REST API Request with paging** action, you can effectively retrieve and handle large datasets from Stratsys while adhering to best practices for performance and API compliance.
