# NetSuite REST API Request with paging

Use the [NetSuite REST API](https://starapi.hogia.se) to retrieve paginated data.

The **REST API request with paging** action enables you to interact with the [NetSuite REST API](https://starapi.hogia.se) to retrieve large, paginated datasets. This action simplifies working with endpoints that return multiple pages of data, such as vouchers, invoices, or accounts. Pagination is managed automatically, allowing you to focus on processing the data.

![NetSuite REST API Paging](/images/flow/netsuite-rest-api-paging.png)  

**Example** ![Example](/images/strz.jpg)

In the example above, a **paginated REST API Request** is used to retrieve accounts and insert them into an SQL Server table. Each result from the **REST API Request** is converted using the [JSON to DataReader](../json/get-json-datareader.md) action, and then inserted using the [SQL Server Insert](../sql-server/insert-data.md) action.

<br/>

## Properties

| Name            | Required | Description          |
|---------------- | -------- | ---------------------|
| Title           | No | The title or name of the request.  |
| Connection      | Yes | The [NetSuite Connection](./connection.md) used to make an authenticated request to the NetSuite REST API. |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| Configuration   | Yes | Specifies the HTTP request to the NetSuite API, including the HTTP method, URL, parameters, and return type. |
| Offset          | No | The starting position for data retrieval. Defaults to `0` if not specified. |
| Limit           | No | The number of items to retrieve per request. Defaults to `1000` if not specified. |
| Max page count  | No | The maximum number of pages to fetch. Defaults to 9999 if not specified.      |
| Description     | No | Additional notes or comments about the action or configuration.               |

<br>

## Returns  

The return type is defined when configuring the action. It is usually the raw JSON response from the API.  
We recommend using the built-in [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md) type because it will include additional information about the response, such as the HTTP status code and error(s).

We recommend dumping the raw response into a data store and using data transformation tools to convert it into a usable format. If the API returns a relatively small dataset (10,000–200,000 records), consider using the [Get JSON DataReader](../json/get-json-datareader.md) to flatten the JSON into a tabular format. The data part of the json result content is e.g. located in the **items** array for SuiteQL results. This allows you to process the data as rows and columns, such as by inserting it directly into a SQL Server table.

<br/>

## Configuration

To define a request to the NetSuite REST API, you can start from a template, or define it manually.
If you press the `New Request` button in the Configuration dialog, you can choose from a set of predefined request templates.  

The NetSuite REST API has to major sub sets:
- [Record/v1 for use in getting records and CRUD operations.](https://system.netsuite.com/help/helpcenter/en_US/APIs/REST_API_Browser/record/v1/2026.1/index.html)
- [SuiteQL/v1 for SQL like queries.](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_156257770590.html)

The API's is large, so the template collection contains only a subset of the available APIs. If you cannot find a template for the request you want to make, you canuse the API documentaion or AI to define the request manually.

![NetSuite New Request](/images/flow/netsuite-new-request.png)

To define a request to the NetSuite REST API, you can start from a template, or define it manually:
1. **Method**: Choose the appropriate HTTP method for your request:  
   - `GET`: Retrieve data from NetSuite (e.g., get accounts or financial years).  
   - `POST`: Create new records (e.g., add a new invoice or customer).  
   - `PUT`: Update existing records (e.g., modify accounting settings).  
   - `DELETE`: Remove records (e.g., delete a customer or invoice).  

2. **URI**: Specify the endpoint for your request. For example:  
   - `accounts`: To manage account records.  
   - `vouchers`: To work with vouchers.  

3. **Headers**: 
   - Authentication is automatically set up from the connection settings.

4. **Parameters**: Add query or body parameters as required by the endpoint. Use variables or fixed values based on your workflow to customize the request and ensure it retrieves or updates the desired data.  

5. **Response Type**: Use the default `HttpResponse<string>` to work with raw JSON data. For large datasets, this approach is recommended to reduce memory usage and improve performance.

<br/>

## Error handling

If the response from the NetSuite REST API is set to [HttpResponse&lt;T&gt;](../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` property. When `IsSuccess` is false, the response has an `ErrorContent` property that relay the error messages from the API call or from internally thrown exceptions. 

For other response types and for severe errors, the action will raise an error that could terminate the Flow unless either the `On Error` port is connected, or it is wrapped in a [Try-Catch](../built-in/try-catch.md) action. 

The `On Error` error handler will be triggered for each request error, allowing you to handle errors individually and preventing Flow from automatically raising an error that might terminate the running process.

<br>

## API Limits  

NetSuite enforces rate limits to maintain stable server performance. If you exceed these limits, the API will return a `429 Too Many Requests` error.  
The action handles this by delaying calls and retrying requests. If the retry limit is reached, an error is returned.


<br/>

## Workflow Example  

1. **Configure the Action**:  
   - Define the API endpoint, such as `accounts` or `vouchers`.  
   - Add any required query parameters, including filters or sorting options.  

2. **Handle the Response**:  
   - Process the data batch-by-batch, or store it for later transformation.  
   - If necessary, handle errors or retry logic within your workflow.  

3. **Post-Processing**:  
   - Use tools like SQL or Python to analyze and transform the retrieved data into a usable format.  

<br/>

By using the **REST API Request with paging** action, you can effectively retrieve and handle large datasets from NetSuite while adhering to best practices for performance and API compliance.

!Notes

