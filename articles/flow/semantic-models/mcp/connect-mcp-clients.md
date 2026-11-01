# Connect an MCP client to the semantic model

When you create a semantic model, Flow automatically provides an MCP endpoint. AI agents can use this endpoint to query the semantic model for data.

To connect an MCP client, such as Claude Code or GitHub Copilot in VS Code, to the endpoint:

1. Open the semantic model.
2. Go to **Model settings**.
3. Under **Connect MCP client**, choose one of the connection options described below.

<br/>

## Connection options

### Connect with OAuth

Use this option when users should sign in with their enterprise account, typically Entra ID / Microsoft 365. OAuth enables per-user data access (row-level security), so users only see the data they have been granted access to in the data source.

- [Connect with Claude Code](./connect-claude-code.md#connect-with-oauth)
- [Connect with VS Code](./connect-vscode.md#connect-with-oauth)

> [!NOTE]
> Row-level security (RLS) must be implemented in the underlying data source, for example SQL Server. Neither the MCP server nor the semantic model enforces RLS itself. They forward the identity of the signed-in user to the target query engine, as specified by the [MCP security context](#mcp-security-context).

<br/>

### Connect with a bearer token or a server-specific URL

Use one of these options for admin users who should have access to all data in the semantic model. These options provide API-level access control only, not data-level or row-level security.

- [Connect with Claude Code](./connect-claude-code.md#connect-with-a-server-specific-url-or-bearer-token)
- [Connect with VS Code](./connect-vscode.md#connect-with-a-server-specific-url-or-bearer-token)

> [!NOTE]
> To use these options, you must have created at least one [API key for the MCP policy](../workspaces/api-keys.md#policy-types).

<br/>

## MCP security context

> [!NOTE]
> This setting only applies when users connect with OAuth.

When an MCP client connects with OAuth, the MCP server forwards the ID of the signed-in user to the underlying database system, for example SQL Server or Azure SQL. Administrators and developers can then use the database's built-in RLS feature to make sure users only see the data they are allowed to see.

### Properties

| Name             | Description |
|------------------|-------------|
| User ID Property | The security context property in the target system that receives the signed-in user's ID from the MCP request. This lets you use the user ID in RLS predicates. See the example below. |

### Example

When the semantic model sits on top of SQL Server or Azure SQL, the MCP server sets the following session context before it runs a query against the datasets in the semantic model:

```sql
-- User ID Property: 'user_id'
EXEC sp_set_session_context @key = N'user_id', @value = @user_id, @read_only = 1;
```

The user ID can now be used in security predicate functions to apply RLS, for example;
```sql
CREATE OR ALTER FUNCTION epm.fn_access_predicate_department
    (@DepartmentID varchar(50))
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN
    SELECT 1 AS access_granted
    FROM dbo.user_access_map AS m
    WHERE m.UserID = CAST(SESSION_CONTEXT(N'user_id') AS varchar(50))
      AND m.DepartmentID = @DepartmentID

CREATE SECURITY POLICY dbo.forecast_finance_metrics_policy
    ADD FILTER PREDICATE dbo.fn_access_predicate_department(DepartmentID)
        ON [dbo.[forecast_finance_metrics]
WITH (STATE = ON);
```
