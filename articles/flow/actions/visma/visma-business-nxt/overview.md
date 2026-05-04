# Visma Business NXT overview

Flow includes built-in support for [Visma Business NXT](https://docs.vismasoftware.no/businessnxtapi/) through its GraphQL API. Unlike the REST-based ERP categories in Flow, Visma Business NXT exposes a GraphQL endpoint, and the integration is built around a single dedicated action that runs GraphQL queries and streams the result as tabular data.

To use the action, you first need a [Visma Business NXT connection](./connection.md) configured with a Client ID, Client Secret, and optionally a Customer ID — credentials are obtained from the [Visma developer portal](https://oauth.developers.visma.com/). The same connection is reused, or you can build one dynamically with [Create Visma Business NXT connection](./create-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Setting up the connection
Set up a static [Visma Business NXT connection](./connection.md) with Client ID, Client Secret, and optionally a Customer ID. Or use [Create Visma Business NXT connection](./create-connection.md) to build one at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters, for example when working with multiple Visma customers from the same flow.

<br/>

#### Querying the GraphQL API
[Get Visma Business NXT data](./get-visma-business-nxt-data.md) executes a GraphQL query against the Visma Business NXT API and returns the result as an IDataReader — a forward-only stream of rows that can be piped directly to a destination such as a SQL Server table through [Insert rows](../../sql-server/insert-data.md). The action is configured through three GraphQL settings: the **GraphQL Expression** itself (with a *Create from template* option as a starting point), optional **Variables** for parameterizing the query, and **Item type mapping** which defines the data types of each field returned, so Flow can convert the response into typed objects for downstream processing.
