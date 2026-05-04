# Xledger overview

Flow includes built-in support for [Xledger](https://xledger.com/) through its GraphQL API. The category provides a single dedicated action for querying Xledger data, plus options for setting up the underlying connection statically or building one dynamically at runtime.

To use the Xledger action, you first need an [Xledger connection](./connecting-to-xledger.md) configured with API keys obtained from your Xledger administrator. A single connection holds both a **Production API key** and a **Test API key** — the **Use Xledger test environment** flag controls which one is used at runtime, so you don't need separate connection objects to switch between sandbox and production. When credentials need to be resolved dynamically — for example, to target different Xledger tenants based on flow input — use [Create connection](./create-connection.md) instead.

<br/>

## Explore

#### Setting up the connection
Set up a static [Xledger connection](./connecting-to-xledger.md) with a Production and (optionally) a Test API key, and use the **Use Xledger test environment** flag to switch between the two during development. The same connection is reused across actions and flows in the same Workspace. Or use [Create connection](./create-connection.md) to build a connection at runtime — useful when API keys come from a secrets manager, when a single flow targets different Xledger tenants based on parameters, or when switching between test and production needs to be controlled programmatically as part of a CI workflow.

<br/>

#### Querying the GraphQL API
[Get Xledger data](./get-xledger-data.md) executes a GraphQL query against the Xledger API and returns the result for use in downstream actions. The action accepts an Xledger connection (or a dynamic connection produced by Create connection) and a Configuration that defines the query and how the response should be interpreted.
