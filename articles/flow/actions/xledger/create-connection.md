# Create Connection

Creates an Xledger connection at runtime using API keys supplied as variables or secrets. Use this instead of a static connection when credentials need to be resolved dynamically — for example, when a single Flow connects to different Xledger tenants based on input parameters.


![Flow: Create Xledger connection → Get data](../../../../images/flow/Xledger-dynamic-connection.png)


**Example** ![Example](../../../../images/strz.jpg)  
This flow uses [Declare Variables](../built-in/declare-variables.md) to initialize the connection parameters, then Creates Xledger connection to dynamically use it in the next step. The connection is passed to [Get Xledger Data](./get-xledger-data.md) node, which retrieves account data via GraphQL. On success, the accounts are inserted into the database, while any failures are surfaced — both handled by [Functions](../built-in/function.md).

## Use-cases

- Your organization manages multiple Xledger tenants and the target tenant is determined at runtime (e.g., per-entity consolidation Flows).
- API keys are stored in a secrets manager and must be injected as variables rather than entered in a static connection dialog.
- You need to switch between production and test environments programmatically as part of a CI/test workflow.

If your Flow always connects to the same Xledger tenant with the same credentials, a static connection created through the [connection dialog](./connecting-to-xledger.md) is simpler and sufficient.

## How it works

1. The action reads its properties at runtime — typically bound to variables sat earlier in the Flow.
2. It builds an Xledger connection object using the provided API keys and environment setting.
3. The connection is stored in a variable (named by **Connection variable name**, defaults to `connection`) that downstream Xledger actions reference.

No network call is made at this step — the connection is validated when a downstream action (e.g., [Get Xledger data](./get-xledger-data.md)) first uses it.



## Prerequisites

- A Production and Test API key for Xledger. See your Xledger administrator if you don't have them.
- If using secrets: the secret must already be configured and accessible via `{{secrets.your_key_name}}` syntax. <!-- TODO: Link to secrets documentation when available. -->

## Properties

| Property | Required | Description |
|----------|----------|-------------|
| **Title** | No | Display label for this action in the Flow editor. Does not affect execution. |
| **Production API key** | Yes | API key for the live Xledger environment. Select a Flow or Workspace variable, or enter a value directly (e.g., `{{secrets.xledger_prod_key}}`). |
| **Test API key** | Yes | API key for the Xledger sandbox. Select a Flow or Workspace variable, or enter a value directly. Required regardless of environment setting, but only used at runtime when **Use Test environment** is `true`. |
| **Use Test environment** | No | Routes requests to the Xledger test environment instead of production. Defaults to `false`. |
| **Connection variable name** | No | The variable name downstream Xledger actions use to reference this connection. Defaults to `connection`. Change this if your Flow creates multiple Xledger connections. |
| **Description** | No | Free-text notes visible only in the Flow editor. |

## Returns

A connection object, stored in the Flow variable specified by **Connection variable name** (defaults to `connection`). Xledger actions reference this variable to authenticate API requests.


> [!TIP]
> Store API keys as secrets rather than hardcoding them. Reference them with `{{secrets.your_key_name}}` in the property field. This keeps credentials out of the Flow definition and makes key rotation easier.

> [!WARNING]
> If **Connection variable name** is left at the default (`connection`) and your Flow creates more than one Xledger connection, the second will silently overwrite the first. Use distinct variable names when working with multiple connections.



## Error handling

| Condition | Behavior |
|-----------|----------|
| Missing **Production API key** | Action fails at runtime. Error surfaces in the Flow execution log. |
| Downstream action uses expired key | The downstream action fails. Update the key in your secrets store and re-run the Flow. |

## Related

- [Connecting to Xledger](./connecting-to-xledger.md) — how to set up a static Xledger connection through the UI dialog
- [Get Xledger data](./get-xledger-data.md) — the action that uses this connection to retrieve data from Xledger