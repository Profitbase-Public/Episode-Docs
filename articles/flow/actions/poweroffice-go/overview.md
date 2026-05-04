# PowerOffice Go overview

Flow includes built-in support for [PowerOffice Go](https://developer.poweroffice.net) through its REST API (v2), letting you read data from a PowerOffice Go organization — customers, accounts, employees, invoices, accounting records — as well as create and update records.

To use any PowerOffice Go action, you first need a [PowerOffice Go connection](./poweroffice-go-connection.md) configured with a Client ID and Client Secret. Some APIs require an additional **Subscription Key**, and a **Use PowerOffice Go test environment** option lets you point the connection at demo endpoints for testing instead of production. The same connection is reused across actions, or you can build one dynamically with [Create PowerOffice Go connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [PowerOffice Go connection](./poweroffice-go-connection.md) with Client ID, Client Secret, optionally a Subscription Key and a flag to use demo endpoints for testing. Or use [Create PowerOffice Go connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters.

<br/>

#### Calling the PowerOffice Go API
Two actions cover the PowerOffice Go REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any PowerOffice Go endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as customer or invoice lists, fetching one page at a time with configurable **Start page** (default 1), **Items per page** (default 5000), and **Max page count** (default 9999). Both actions support starting from built-in request templates or defining requests manually. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.
