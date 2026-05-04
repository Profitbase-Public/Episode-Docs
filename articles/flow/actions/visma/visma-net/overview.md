# Visma.Net overview

Flow includes built-in support for [Visma.Net](https://integration.visma.net/API-index/) through its REST API, letting you read data from a Visma.Net subscription — customers, projects, invoices, accounting records — as well as create and update records.

To use any Visma.Net action, you first need a [Visma.Net connection](./visma-net-connection.md) configured with a Tenant ID, Client ID, Client Secret, and optionally a default Company ID. The same connection is reused across actions, or you can build one dynamically with [Create Visma.Net connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Visma.Net connection](./visma-net-connection.md) with Tenant ID, Client ID, Client Secret, and optionally a default Company ID that requests will target unless overridden per action. Or use [Create Visma.Net connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Visma.Net subscriptions from the same flow.

<br/>

#### Calling the Visma.Net API
Two actions cover the Visma.Net REST API, depending on whether the endpoint returns paginated data. [REST API Request](./rest-api-request.md) calls any Visma.Net endpoint — both for reading data (`GET`) and modifying it (`POST`, `PUT`, `DELETE`). [REST API Request with paging](./paged-rest-api-request.md) handles paginated endpoints such as customer or invoice lists, fetching one page at a time with configurable **Start page** (default 1), **Items per page** (default 1000), and **Max page count** (default 9999). Both actions support starting from built-in request templates or defining requests manually, and both accept an optional per-action **Company Id** that overrides the default set in the connection. For large data sets, the recommended pattern is to dump the raw JSON response into a data store and transform it afterwards.
