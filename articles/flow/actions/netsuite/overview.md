# NetSuite overview

Flow includes built-in support for [NetSuite](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/book_1559132836.html) through its REST API, letting you read data from a NetSuite instance —  accounts, customers, employees, transactions — as well as create and update records.

To use any NetSuite action, you first need a [NetSuite connection](./connection.md) configured with an Account ID, API consumer key, Client credentials certificate ID, and Private certificate key. The same connection is reused across actions, or you can build one dynamically with [Create NetSuite connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [NetSuite connection](./connection.md) with an Account ID, API consumer key, Client credentials certificate ID, and Private certificate key (PEM), or use [Create NetSuite connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple NetSuite organizations from the same flow.

<br/>

#### Calling the NetSuite API
Two actions cover the NetSuite REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any NetSuite endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as transactions, using configurable **Offset** (default `0`) and **Limit** (default `1000`). Both actions support starting from built-in request templates or defining requests manually. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.

<br/>

#### Introduction to SuiteQL 
[SuiteQL](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_156257770590.html) is NetSuite's SQL-like query language for retrieving data from NetSuite records using familiar SELECT statements. It is useful when you need to query related data in a single request, apply filters, sort result sets, or control which columns are returned.

You can use SuiteQL with the NetSuite REST API actions by sending a request to the SuiteQL query endpoint and passing your query in the request body. In most cases, this is the preferred approach for reporting-style reads, while standard REST record endpoints are better for create and update operations.

When working with larger datasets, combine SuiteQL with pagination in your flow to process data in predictable batches.

<br/>

#### Introduction to the Record API
The [NetSuite Record API](https://system.netsuite.com/help/helpcenter/en_US/APIs/REST_API_Browser/record/v1/2026.1/index.html) is designed for direct CRUD operations on individual records such as customers, vendors, items, and transactions. Use it when your flow needs to create new records, update specific fields, fetch a single record by ID, or delete records.

Compared to SuiteQL, the Record API is usually the better choice for transactional operations and business process automation, while SuiteQL is better for analytical reads across many records.

A common pattern in Flow is to use SuiteQL to find the record IDs you need, then call the Record API to read full record details or apply updates.
