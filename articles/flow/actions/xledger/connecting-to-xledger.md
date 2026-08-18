# Connecting to Xledger


Before any Xledger action in a Flow can retrieve data, it needs a connection that authenticates against the Xledger API. This page covers how to create and configure that connection.



## Prerequisites

- An Xledger API key for the Production environment. Contact your Xledger administrator if you don't have one.
- If you plan to test against the sandbox, a separate API key for the Xledger test environment.
- Familiarity with [Workspace Objects](../../workspaces/workspace-objects.md) — Xledger connections are stored as Workspace Objects and can be reused across actions and Flows within the same Workspace.



## How to create a connection

1. Add any Xledger action to your Flow (e.g., **Get Xledger data**).
2. In the action's property panel, click the **Connection** dropdown and select an existing connection or [create a new one](create-connection.md).
   The **Get Xledger data connection** dialog opens.
3. Enter a **Name** for the connection. This label appears in the Connection dropdown across all Xledger actions in the Solution, so choose something identifiable (e.g., "Xledger Production — Finance").
4. Enter your **API key for Production environment**.
5. Click **Test connection** to validate the credentials before saving.

> [!WARNING]
> If the test fails, the connection can still be saved — but any Flow that uses it will fail at runtime. Always verify the connection before using it in a scheduled or production Flow.


7. Click **OK** to save.

To reuse an existing connection instead of creating one, click **Use existing connection** at the top of the dialog and select from the dropdown.



## Updating or rotating API keys

To update an API key, open any Xledger action that uses the connection, click the **Connection** dropdown, and edit the existing connection. Replace the key and click **Test connection** to verify, then save.



> [!WARNING]
> If you rotate an API key in Xledger without updating the connection in Profitbase, all Flows using that connection will fail at their next execution.




## Related

- [Xledger Developer Portal](https://xledger.com/)
- [Create connection](./create-connection.md) — how to set up and configure the Xledger API connection used by all Xledger actions in a Flow
- [Get Xledger data](./get-xledger-data.md) — the action that uses this connection to read data from Xledger
- [Hypergene Flow Workspace Objects](../../workspaces/workspace-objects.md)