# MCP security context

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
