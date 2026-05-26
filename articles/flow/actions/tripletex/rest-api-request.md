# REST API Request

Use [Tripletex REST APIs (v2)](https://tripletex.no/v2-docs/) to read or write data.

The **REST API Request** action sends a single HTTP request to the Tripletex API and returns the response. Use it to read individual records (a customer by ID, a single invoice), to create or update records (`POST` a new contact, `PUT` updated project details), or to call any Tripletex endpoint that returns a manageable amount of data in one response.

For endpoints that paginate large result sets (lists of customers, invoices, ledger postings across a date range), use [REST API Request with paging](./paged-rest-api-request.md) instead — that action handles the page-by-page iteration automatically.

![flow fetches the latest currency exchange](../../../../images/flow/tripletex.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow fetches the latest currency exchange rate from Tripletex and displays it. **Fetch Latest Currency Rate** is a REST API Request action configured against the Tripletex `currency/exchangeRate` endpoint, with the target currency code passed in through the `URI` input port. On success, the response is exposed on the `response` output port and bound to the `item` input of [Display Currency Rate](../built-in/function.md), which formats the rate for the user. If the request fails — Tripletex unreachable, 401 from expired tokens, 404 from an unknown currency code — the **Error** port routes to [Show Error: Failed to Retrieve Data](../built-in/throw-exception.md), surfacing the failure with a readable message rather than letting the Flow terminate silently. This pattern — single request, Success/Error split, named exception handler — is the smallest robust shape for any Tripletex read operation.

## Use cases

- Reading a single record by ID (customer, project, invoice, voucher).
- Creating a new record (`POST` to `customer`, `order`, `invoice`).
- Updating an existing record (`PUT` to `customer/{id}`, `project/{id}`).
- Deleting a record (`DELETE` to any endpoint that supports it).
- Calling a non-list endpoint that returns a manageable response (`currency/exchangeRate`, `token/session/:create`, `company/>whoAmI`).

For list endpoints that return paged data, use [REST API Request with paging](./paged-rest-api-request.md).

## Properties

| Property | Required | Description |
|---|---|---|
| **Title** | No | Display name of the action node on the Flowchart. Defaults to `REST API Request`. The current value is appended to the label of the **Edit configuration** field (`Edit configuration - {Title}`). |
| **Connection** | No | The [Tripletex Connection](./tripletex-connection.md) used at runtime, unless **Enable dynamic connection** is on. When the toggle is on, **Connection** is used at design time only — for endpoint autocomplete and request validation. |
| **Enable dynamic connection** | No | Toggle that switches the action between static and dynamic connection mode. Off: the action uses **Connection**. On: **Connection** becomes design-time-only and **Dynamic connection** controls the runtime connection. |
| **Dynamic connection** | No | Visible and required only when **Enable dynamic connection** is on. Bind it to a `Connection` variable produced by the [Create Tripletex Connection](./create-connection.md) action. |
| **Edit configuration - {Title}** | No | The HTTP request to send — method, URI, parameters, response type. Click the field to open the request editor; pick a template from **New Request** or define the request manually. The field summary in the property panel shows the HTTP method and `Configured (edit for details)` once set. |
| **Disabled** | No | When checked, the action is skipped at runtime and execution flows straight from its input port to its Success output. Use during development to bypass a step without removing it from the canvas. |
| **Description** | No | Free-text notes about the action. Not used at runtime. |




## Returns

The return type is set in **Configuration**. The two options are:

- A **custom data type** defined when configuring the request — useful when the API response shape is known and stable.
- The raw JSON response from the API, wrapped in [`HttpResponse<T>`](../../api-reference/built-in-types/http-response.md) — the default and recommended choice.

`HttpResponse<T>` exposes:

- The raw response body.
- The HTTP status code.
- An `IsSuccess` flag and an `ErrorContent` property populated on failure.

Use `HttpResponse<string>` for large or variable-shape responses and parse the JSON downstream with [Get JSON DataReader](../json/get-json-datareader.md) or a SQL Server target table. Allocating a custom strongly-typed result for every row of a large response measurably hurts performance.

## Configuration

### Defining a request

Open **Configuration** in the property panel. You can start from a template or define the request manually.

**From a template:** click **New Request** and pick a predefined template for one of the commonly used Tripletex endpoints. The template fills in the HTTP method, URI, and response type; adjust parameters as needed.

**Manually:**

1. **Method** — pick the HTTP verb:
    - `GET` to retrieve data.
    - `POST` to create a record.
    - `PUT` to update an existing record.
    - `DELETE` to remove a record.
2. **URI** — the Tripletex endpoint, for example `customer/{id}`, `project`, `invoice/{id}`. Parameters supplied at runtime (for example a customer ID resolved by an upstream action) come in through input ports on the action; reference them in the URI using the variable picker that appears when the URI field has focus.
3. **Headers** — authentication is set automatically from the connection. Add other headers only when the specific endpoint requires them.
4. **Parameters** — query string or body parameters as required by the endpoint.
5. **Response Type** — defaults to `HttpResponse<string>`. Change to a custom type only when the response shape is known and the response is small.

For endpoint-level details (paths, parameters, response models), refer to the [Tripletex API documentation](https://tripletex.no/v2-docs/).

![Dynamics365 Bc New Request](/images/flow/dynamics365-bc-new-request.png)

<br/>

## Error handling

When **Response Type** is [`HttpResponse<T>`](../../api-reference/built-in-types/http-response.md), the response object includes an `IsSuccess` flag and an `ErrorContent` property. On failure, `IsSuccess` is `false` and `ErrorContent` holds the error message from the Tripletex API or from an internally thrown exception. The action does not raise an error in this case — downstream actions can branch on `IsSuccess`.

For other response types and for severe errors (connection refused, malformed response, etc.), the action raises an error that terminates the Flow unless either:

- The **Error** output port is connected to a handler (typically [Throw exception](../built-in/throw-exception.md) or a logging action).
- The action is wrapped in a [Try-Catch](../built-in/try-catch.md) block.

Connecting the **Error** port is the more common pattern — it lets the Flow continue on success while routing failures to a single exception node.

## API limits

Tripletex enforces rate limits to control server load. When the limit is exceeded, the API returns `429 Too Many Requests`. The action retries automatically (three attempts) before surfacing the error.

To avoid hitting the limit:

- Optimise queries — request only the fields needed via the `fields` query parameter, and narrow result sets with date and ID filters where possible.
- Batch related writes into a single request where the endpoint supports it (for example `POST /customer/list`).
- Monitor API usage in Tripletex's developer portal and stagger high-volume Flows away from each other.

See [Tripletex integration best practices](https://developer.tripletex.no/docs/documentation/integration-best-practices/) for endpoint-level guidance.

## See also

- [REST API Request with paging](./paged-rest-api-request.md) — for endpoints that return paged result sets.
- [Tripletex Connection](./tripletex-connection.md) — the static connection used by this action.
- [Create Tripletex Connection](./create-connection.md) — builds a connection at runtime for use via the **Dynamic connection** property.
- [Get JSON DataReader](../json/get-json-datareader.md) — parses the raw JSON response into rows and columns.
- [Tripletex API documentation](https://tripletex.no/v2-docs/) — endpoint reference for Tripletex REST API v2.


