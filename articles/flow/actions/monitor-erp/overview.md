# Monitor ERP overview

Flow includes built-in support for [Monitor ERP](https://api.monitor.se/) through its REST API, letting you read data from a Monitor ERP organization — vouchers, accounts, customer invoices, employees, financial years — as well as create and update records.

To use any Monitor ERP action, you first need a [Monitor ERP connection](./connection.md). The same connection is reused across actions, or you can build one dynamically with [Create Monitor ERP connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection

Set up a static [Monitor ERP connection](./connection.md) with host, company numbers, and credentials, or use [Create Monitor ERP connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Monitor ERP companies from the same flow.

<br/>

#### Calling the Monitor ERP API

Two actions cover the Monitor ERP REST API, depending on whether the endpoint returns paginated data. 

[REST API Request](./rest-api-request.md) calls any Monitor ERP endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). 

[REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as voucher or invoice lists, fetching one page at a time with configurable **Page** (default 1), **Page size** (default and maximum 500), and **Max page count** (default 9999). Both actions support starting from built-in request templates or defining requests manually. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.
