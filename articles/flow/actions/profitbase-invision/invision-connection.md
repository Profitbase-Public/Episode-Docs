# InVision connection

The **InVision connection** defines how a flow authenticates and communicates with an InVision environment. It is a reusable configuration that can be shared across multiple actions and flows, avoiding repeated credential setup and ensuring consistent access behavior.

A connection is required by most InVision-related flow actions, including read, write, and delete operations.


## Use cases connection

Use an InVision connection when your flow needs to:

- Access InVision data stored in a SQL database  
- Call the InVision API directly  
- Perform operations such as insert, update, delete, or query on InVision-managed objects  


## Creating a new connection

When adding an action that requires an InVision connection, you can either select an existing connection or create a new one.

Enable **Create new connection** to define a new connection object.

### Common properties

| Name | Required | Description |
|------|----------|-------------|
| Name | Yes | Logical name of the connection object. Used for identification inside the solution. |
| Solution Id | Yes | The ID of the InVision solution this connection belongs to. |



## Connection types

An InVision connection can be configured in one of two modes:

- **Database connection**
- **API connection**

Choose the tab that matches how your flow should communicate with InVision.



## Database connection

The **Database connection** mode is used when the flow interacts directly with the InVision SQL database.

This is typically required for actions that operate on tables or views, such as delete, insert, update, or data retrieval actions.

### Database connection properties

| Name | Required | Description |
|------|----------|-------------|
| Server name | Yes | Name or address of the SQL Server hosting the InVision database. |
| Database name | Yes | Name of the target database. |
| Username | Yes | Database user name used for authentication. |
| Password | Yes | Password for the specified database user. |

Use **Test connection** to verify that the credentials and server settings are valid before saving the connection.



## API connection

The **API connection** mode is used when the flow communicates with InVision through its HTTP API instead of direct database access.

This mode is typically used for scenarios where database access is restricted or when operations must go through the public API surface.

### API connection properties

| Name | Required | Description |
|------|----------|-------------|
| API endpoint | Yes | Base URL of the InVision API endpoint. |
| API Key | Yes | API key used to authenticate requests against the InVision API. |

Authentication is handled using API key authentication. The key is sent with each request initiated by the flow.

Use **Test connection** to confirm that the endpoint and API key are valid.



## Reuse and scope

Once created, an InVision connection can be reused by multiple flow actions within the same solution. This ensures:

- Centralized credential management  
- Consistent access configuration  
- Easier maintenance when connection details change  

If connection details need to be updated, changes are automatically applied to all actions that reference the connection.



## Notes and best practices

- Use database connections only when direct database access is required  
- Prefer API connections in environments with stricter security boundaries  
- Name connections clearly to reflect their target environment and purpose  
- Always test the connection before saving to avoid runtime failures  
