# MCP Server endpoints

A Flow with an [MCP tool trigger](../triggers/mcp/mcp-tool-trigger.md) can be called by MCP clients, like VS code and Claude Desktop.  
In the Designer, you can find the available MCP API endpoints of a Workspace in the `Resources -> Flow Properties` window, or you can add an [MCP tool trigger](../triggers/mcp/mcp-tool-trigger.md) to the Flow and get them from the `Property panel` with the trigger selected.  

Flow exposes an MCP server pr [Workspace](../workspaces.md) and [Environment](../environments.md), meaning you need one MCP client pr endpoint (Workspace and Environment). 

All endpoints requires some type of authorization to access. Flow currently supports the following 3 options:

![MCP Server Endpoints](/images/flow/mcp-server-endpoints.png)

<br/>

### Connect with Bearer token
Your MCP client must include an API key in the `Authorization` header when making the request.  
Example url: `https://app.profitbase.biz/hypotesia/mcp/tenants/123/workspaces/456/environments/production/s`  
Headers: `Authorization: Bearer [token]`  
<br/>

> [!NOTE]
> To enable this option, you must configure at least one [API key](../tenants/api-keys.md) at the tenant level.  

<br/>

### Connect with server-specific URL
Your client must include the API key in the URL. This is the easiest, but least secure way to call the API. 
Example url: `https://app.profitbase.biz/hypotesia/mcp/tenants/123/workspaces/456/environments/production/s/my-api-key`  

> [!NOTE]
> To enable this option, you must configure at least one [API key](../tenants/api-keys.md) at the tenant level.  

<br/>

### Connect with OAuth
Before you can use this option, you must configure the [MCP Server Settings](../workspaces/mcp-server-settings.md) for your tenant in the Portal.  
<br/>
OAuth 2.1 is the official authentication method supported by the MCP protocol. It requires obtaining an access token from an Authorization Server (such as Microsoft Entra ID, Auth0 or Scalekit) and including it as a bearer token in the `Authorization` header for all requests to the (Flow) MCP Server APIs.  

Example url: `https://app.profitbase.biz/hypotesia/mcp/tenants/123/workspaces/456/environments/production`



