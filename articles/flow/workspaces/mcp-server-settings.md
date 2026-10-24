# MCP Server Settings

To enable MCP Clients to use OAuth authorization to Flow MCP Server APIs and [Semantic models](../semantic-models/connect-mcp-clients.md), you need to configure the MCP Server Settings for the Workspace.
For example, if you try to use the OAuth endpoint of an [MCP Trigger](../triggers/mcp/mcp-tool-trigger.md) without configuring the MCP Server Settings, you will get an `unauthorized` error response from the API. 

![MCP Server Settings](/images/flow/mcp-server-settings.png)

<br/>

## Get started - use default Authorization Server
The quickest way to enable OAuth for Flow MCP Server APIs is to use the Authorization Server associated with your Flow instance. 

1. In the `MCP Server Settings` window, select the `Authorization Server` tab.
2. Click `Create default settings`. This will enable OAuth authorization for your tenant. The Authority property should now contain something like `https://login.microsoftonline.com/GUID/v2.0`

## Registering MCP Server Apps
Different Authentication Servers, such as Entra ID, Auth0, Keycloak etc has varying support for the MCP authentication protocol. To support OAuth authentication, an MCP Server such as Flow MUST provide an `Authorization Server` and optionally `Scopes` (Entra ID _requires_ scopes, others may not). 

### Properties
| Name             | Description                     |
|------------------|---------------------------------|
| Application name | The display name of the MCP Server App. Used only as a label. |
| Client ID        | The Client ID as registered in the Authorization Server. This is usually a GUID. Required if using Entra ID as Authorization Server. |
| Scope            | Required if using Entra ID as Authorization Server. |

### Microsoft Entra ID

1. Create an App Registration in Entra ID.
2. 

> [!NOTE] 
> Currently, Microsoft Entra ID does not support Dynamic Client Registration. This means that you have to obtain an access token (using the Client Credentials flow) from Entra ID before calling the Flow MCP Server endpoint. If you want to use the Flow MCP capabilities in a client that only support Dynamic Client Registration, you need to use your own Autorization Server instead (like explained below.)

<br/>

## Use your own Authorization Server
If you want to use your own Authorization Server (for example Keycloak, Auth0 or Scalekit), or you want to use your own Entra ID instance to do authorization, do the following:

1) In the `MCP Server Settings` window, select the `Authorization Server` tab.
2) Enter the address of your Authorization Server in the `Authority` field (remember to `save` the changes).