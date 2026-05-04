# Fortnox overview

Flow includes built-in support for [Fortnox](https://www.fortnox.se/) through its REST API, letting you read data from a Fortnox tenant — customers, invoices, accounts, employees, financial years — as well as create and update records. The category also includes a dedicated action for retrieving SIE files, the Swedish standard format for transferring accounting data between systems.

To use any Fortnox action, you first need a [Fortnox connection](./connection.md) configured with a Client ID, Client Secret, Tenant ID, and one or more [scopes](https://www.fortnox.se/developer/guides-and-good-to-know/scopes) selecting which APIs the connection is allowed to call. The same connection is reused across actions, or you can build one dynamically with [Create Fortnox connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Fortnox connection](./connection.md) with credentials and scopes, or use [Create Fortnox connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters. The first time you connect, Fortnox requires going through an activation flow to obtain an access code; the connection page covers this step by step.

<br/>

#### Calling the Fortnox API
Two actions cover the Fortnox REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any Fortnox endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as customer or invoice lists, fetching one page at a time with configurable **Offset** (default 0), **Items per page** (default 5000), and **Max page count** (default 9999). Both actions support starting from built-in request templates or defining requests manually, and both handle Fortnox's `429 Too Many Requests` rate limits automatically by delaying and retrying. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.

<br/>

#### Retrieving SIE files
[Get SIE file stream](./get-sie-file-stream.md) downloads a [Standard Import Export (SIE)](https://sie.se/in-english/) file from Fortnox as a stream — useful for retrieving balances, transactions, and vouchers in the Swedish accounting standard format. You select which SIE type to retrieve (1–4, depending on the level of detail you need) and which financial year. The resulting stream is typically passed to a [Load SIE file](../sie/load-file.md) action for parsing, then inserted into a database table.
