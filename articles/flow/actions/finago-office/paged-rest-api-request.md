# Finago Office REST API Request with paging

Use the [Finago Office (24SevenOffice) API](https://rest-api.developer.24sevenoffice.com/doc/v1/) to retrieve paginated data.

The **REST API request with paging** action enables you to interact with the **Finago Office REST API** to retrieve large, paginated datasets. This action simplifies working with endpoints that return multiple pages of data, such as transaction lines. Pagination is managed automatically, allowing you to focus on processing the data.

![Finago Office REST API Paging](/images/flow/finagooffice-rest-api-paging.png)  

**Example** ![Example](/images/strz.jpg)

In the example above, a **paginated REST API Request** is used to retrieve transaction lines and insert them into an SQL Server table. Each result from the **REST API Request** is converted using the [JSON to DataReader](../json/get-json-datareader.md) action, and then inserted using the [SQL Server Insert](../sql-server/insert-data.md) action.

<br/>

## Properties

| Name            | Required | Description          |
|---------------- | -------- | ---------------------|
| Title           | No | The title or name of the request.  |
| Connection      | Yes | The [Finago Office Connection](./connection.md) used to make an authenticated request to the Finago Office REST API. |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| Configuration   | Yes | Specifies the HTTP request to the Finago Office API, including the HTTP method, URL, parameters, and return type. |
| Start page   | No | Offset to the starting page for data retrieval. Defaults to 1 if not specified. |
| Items per page  | No | The number of items to retrieve per page. Defaults to 50 if not specified.  |
| Max page count  | No | The maximum number of pages to fetch. Defaults to 9999 if not specified.      |
| Description     | No | Additional notes or comments about the action or configuration.               |

<br>

## Returns  

The return type is defined when configuring the action. It can be a custom data type set by the template, or the raw JSON response from the API.  
We recommend using the built-in [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) type because it will include additional information about the response, such as the HTTP status code and error(s).

We recommend dumping the raw response into a data store and using data transformation tools to convert it into a usable format. If the API returns a relatively small dataset (10,000–200,000 records), consider using the [Get JSON DataReader](../json/get-json-datareader.md) to flatten the JSON into a tabular format. This allows you to process the data as rows and columns, such as by inserting it directly into a SQL Server table.

<br/>

## Configuration

To define a request to the Finago Office REST API, you can start from a template, or define it manually.
If you press the `New Request` button in the Configuration dialog, you can choose from a set of predefined request templates.  

The Finago Office REST API is large, so the template collection contains only a subset of the available APIs. If you cannot find a template for the request you want to make, you can refer to the [Finago Office API documentation](https://rest-api.developer.24sevenoffice.com/doc/v1/) and define the request manually.

![New Request](/images/flow/dynamics365-bc-new-request.png)

To define a request to the Finago Office REST API, you can start from a template, or define it manually:
1. **Method**: Choose the appropriate HTTP method for your request:  
   - `GET`: Retrieve data from Finago Office (e.g., get transaction lines).  

2. **URI**: Specify the endpoint for your request. For example:  
   - `transaction lines`: To manage transaction lines records.

3. **Headers**: 
   - Authentication is automatically set up from the connection settings.

4. **Parameters**: Add query or body parameters as required by the endpoint. Use variables or fixed values based on your workflow to customize the request and ensure it retrieves or updates the desired data.  

5. **Response Type**: Use the default `HttpResponse<string>` to work with raw JSON data. For large datasets, this approach is recommended to reduce memory usage and improve performance.

<br/>

## Error handling

If the response from the Finago Office REST API is set to [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` property. When `IsSuccess` is false, the response has an `ErrorContent` property that relays the error messages from the API call or from internally thrown exceptions. 

For other response types and for severe errors, the action will raise an error that could terminate the Flow unless either the `On Error` port is connected, or it is wrapped in a [Try-Catch](../built-in/try-catch.md) action. 

The `On Error` error handler will be triggered for each `page error`, allowing you to handle errors individually and preventing Flow from automatically raising an error that might terminate the running process.

<br>

## API Limits  

Finago Office enforces rate limits to maintain stable server performance. If you exceed these limits, the API will return a `429 Too Many Requests` error.  
The action handles this by delaying calls and retrying requests. If the retry limit is reached, an error is returned.


<br/>

## Workflow Example  

1. **Configure the Action**:  
   - Define the API endpoint, such as `transaction lines`.  
   - Add any required query parameters (eg. from and to date), including filters or sorting options.  

2. **Handle the Response**:  
   - Process the data page-by-page, or store it for later transformation.  
   - If necessary, handle errors or retry logic within your workflow.  

3. **Post-Processing**:  
   - Use tools like SQL or Python to analyze and transform the retrieved data into a usable format.  

<br/>

By using the **REST API Request with paging** action, you can effectively retrieve and handle large datasets from Finago Office while adhering to best practices for performance and API compliance.
