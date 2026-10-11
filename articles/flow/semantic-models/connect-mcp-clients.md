# Connect an MCP client to the semantic model

When you create a semantic model, Flow automatically provides an MCP endpoint. AI agents can use this endpoint to query the semantic model for data.

To connect an MCP client such as Claude Desktop or Microsoft 365 Copilot to the endpoint:

1. Open the semantic model.
2. Go to **Model settings**.
3. Under **Connect MCP client**, choose one of the connection options described below.

<br/>

## Connection options

**Connect with OAuth**  
Use this option when users should sign in with their enterprise account, typically Entra ID / Microsoft 365. OAuth enables per-user data access (row-level security), so users only see the data they have been granted access to in the data source.

> [!NOTE]
> Row-level security (RLS) must be implemented in the underlying data source, for example SQL Server. Neither the MCP server nor the semantic model enforces RLS itself. They forward the identity of the signed-in user to the target query engine, as specified by the [MCP security context](#mcp-security-context).

**Connect with bearer token** or **Connect with server-specific URL**  
Use one of these options for admin users who should have access to all data in the semantic model. These options provide API-level access control only, not data-level or row-level security.

> [!NOTE]
> To use this option, you must have created at least one [API Key for the MCP Policy](../workspaces/api-keys.md#policy-types)

<br/>

## MCP security context

> [!NOTE]
> This setting only applies when users connect with OAuth.

When an MCP client connects with OAuth, the MCP server forwards the user ID of the signed-in user to the underlying database system, for example SQL Server or Azure SQL. Administrators and developers can then use the database's built-in RLS feature to make sure users only see the data they are allowed to see.

### Properties

| Name             | Description |
|------------------|-------------|
| User ID Property | The security context property in the target system that receives the signed-in user's ID from the MCP request. This enables the user id to be used in **RLS (row-level security)** predicates. See example below. |

### Example

When the semantic model sits on top of SQL Server or Azure SQL, the MCP server sets the following session context before it runs a query against the datasets in the semantic model: 

```sql
-- User ID Property: 'user_id'
EXEC sp_set_session_context @key = N'user_id', @value = @user_id, @read_only = 1;
```
The user id can now be used in **RLS (row-level security)** prediates.