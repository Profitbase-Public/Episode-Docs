# Tripletex overview

Flow includes built-in support for [Tripletex](https://tripletex.no/viktig-informasjon/api/) through its REST API (v2), letting you read data from a Tripletex company — customers, projects, invoices, accounting records — as well as create, update, and delete records.

To use any Tripletex action, you first need a connection configured with two tokens: a **Consumer token** (the registered API consumer application) and an **Employee token** (the user the API calls act on behalf of). You can also set an optional **Default company Id** for multi-company setups, and a **Use Tripletex test environment** flag to point the connection at the sandbox during development. Configure the connection once and reuse it across actions, or build one dynamically when credentials live outside Flow.

<br/>

## Explore

#### Tripletex Connection
A static connection configured once in the workspace and reused across Flows. Use this when the same credentials apply to every run.
[Read more](./tripletex-connection.md)
<br/>

#### Create Tripletex Connection
Builds a `Connection` at runtime from credentials supplied as inputs. Use this when credentials live in an external store, or when the Flow needs to choose between Tripletex subscriptions per execution.
[Read more](./create-connection.md)
<br/>

#### REST API Request
Calls any Tripletex endpoint with `GET`, `POST`, `PUT`, or `DELETE`. Use it to read single records, create or update data, or call non-list endpoints (`currency/exchangeRate`, `token/session/:create`).
[Read more](./rest-api-request.md)
<br/>

#### REST API Request with paging
Iterates over a paged Tripletex endpoint automatically, exposing one page at a time. Use it for list endpoints (`/customer`, `/invoice`, `/project`, `/voucher`) where requesting everything in one call would time out or exceed memory.
[Read more](./paged-rest-api-request.md)
<br/>