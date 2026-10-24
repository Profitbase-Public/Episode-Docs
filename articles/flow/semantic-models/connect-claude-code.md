# Connect Claude Code to a Semantic Model MCP Server

This article describes how to connect Claude Code to a Semantic Model MCP server.

## Connect with a server-specific URL or bearer token
This option is intended for machine-to-machine communication, and for admins and developers who are developing or testing MCP services in Flow. 

1. In Flow, open a **Semantic Model**.
2. Go to **Model Settings** -> **Connect MCP Client** and copy the url in the **Connect with server specific URL** tab.
3. In a terminal, run:

```
claude mcp add --transport http my-server-name "url"
```

**Example**  
```
claude mcp add --transport http financial-planning "https://app5.profitbase.biz/hypotesia/corellia/mcp/tenants/11111-2222-3333-4444-8E404DD68B96/workspaces/F55AD6B0-0000-0000-0000-50000D67D792/semantic-models/867dce80-0000-0000-0000-daef229856b2/s/[token]"
```

<br/>

## Connect with OAuth
This option is intended for MCP clients, such as Claude or Copilot, that operate on behalf of users.

### Microsoft Entra ID as authorization server

#### Step 1: Create an Entra ID app registration and a Flow MCP Server App
1. Create an Entra ID app registration by following [these steps](../workspaces/mcp-server-settings.md#step-1-create-the-app-registration).
2. Create an MCP Server App by following [these steps](../workspaces/mcp-server-settings.md#step-5-update-the-flow-mcp-server-app).

#### Step 2: Enable Claude Code sign-in
1. Sign in to the [Azure portal](https://portal.azure.com).
2. Go to **Entra ID** -> **App registrations** and open the app registration you created in step 1.
3. Go to **Authentication** -> **Settings**, enable **Allow public client flows**, and click **Save**.
4. Go to **Authentication** -> **Redirect URI configuration** and click **Add redirect URI**.
5. Select **Mobile and desktop applications** and enter `http://localhost:8080/callback` as the redirect URI. This is the callback URL used by Claude Code running on your computer. The command in the next step specifies port 8080. If that port is already in use on your machine, choose a different port and use it in both places.

#### Step 3: Add the MCP server to Claude Code
In a terminal, run the command below, and replace the placeholders:

- `my-server-name`: a descriptive name, such as `financial-planning`.
- `https://app.domain.com/mcp/tenants/...`: the URL of your Semantic Model MCP service.
- `<app-client-id>`: the **Application (client) ID** of the Entra ID app registration.

```
claude mcp add --transport http my-server-name "https://app.domain.com/mcp/tenants/..." --client-id <app-client-id> --callback-port 8080
```
