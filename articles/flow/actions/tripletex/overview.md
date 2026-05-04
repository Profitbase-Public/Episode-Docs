# Tripletex overview

Flow includes built-in support for [Tripletex](https://tripletex.no/viktig-informasjon/api/) through its REST API (v2), letting you read data from a Tripletex organization — customers, projects, invoices, accounting records — as well as create and update records.

To use any Tripletex action, you first need a [Tripletex connection](./tripletex-connection.md) configured with two tokens: a **Consumer token** (which authenticates the registered API consumer application) and an **Employee token** (which identifies the employee on whose behalf the API calls are made). You can also set an optional **Default company Id** to target a specific company by default, and a **Use Tripletex test environment** flag to point the connection at the sandbox during development. The same connection is reused across actions, or you can build one dynamically with [Create Tripletex connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Tripletex connection](./tripletex-connection.md) with Consumer token, Employee token, optionally a Default company Id and a flag to use the sandbox environment. Or use [Create Tripletex connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Tripletex organizations from the same flow.

<br/>

#### Calling the Tripletex API
Two actions cover the Tripletex REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any Tripletex endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as customer or invoice lists, fetching one page at a time with configurable **Start index** (default 0), **Items per page** (default 5000), and **Max page count** (default 9999). Both actions support starting from built-in request templates or defining requests manually. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.
