# Xledger overview

Flow includes built-in support for [Xledger](https://xledger.com/) through its GraphQL API. The category provides a single dedicated action for querying Xledger data, plus options for setting up the underlying connection statically or building one dynamically at runtime.

To use the Xledger action, you first need an [Xledger connection](./connecting-to-xledger.md) configured with API keys obtained from your Xledger administrator. A single connection can hold the API key for all Xledger environments. To switch between Xledger environments use the **Environment** dropdown in the [connection](create-connection.md) dialog. When credentials need to be resolved dynamically — for example, to target different Xledger tenants based on flow input — use [Create connection](./create-connection.md) instead.

<br/>

## Explore

#### Setting up the connection
Set up a static [Xledger connection](./connecting-to-xledger.md) with a Production, Test (optional) and Demo (optional) API keys. The same connection is reused across actions and flows in the same Workspace. Or use [Create connection](./create-connection.md) to build a connection at runtime — useful when API keys come from a secrets manager, when a single flow targets different Xledger tenants based on parameters, or when switching between test and production needs to be controlled programmatically as part of a CI workflow.

<br/>

#### Querying the GraphQL API
[Get Xledger data](./get-xledger-data.md) executes a GraphQL query against the Xledger API and returns the result for use in downstream actions. The action accepts an Xledger connection (or a dynamic connection produced by Create connection) and a Configuration that defines the query and how the response should be interpreted.
