# Connect VS Code to a Semantic Model MCP Server

This article describes how to connect Claude Code to a Semantic Model MCP server.

## Connect with a server specific URL or bearer token
This option is intended for machine-to-machine communication, and for admins and developers who are developing or testing MCP services in Flow. 

1. In Flow, open a **Semantic Model**.
2. Go to **Model Settings** -> **Connect MCP Client** and copy the url in the **Connect with server specific URL** tab.
3. Open VS Code and press **Ctrl + Shift + p**.
4. Select or enter **MCP: Add server...**
5. Select **HTTP**
6. Paste the url you copied in `step 2` above and complete the remaining steps.

<br/>

## Connect with OAuth
This option is intended for MCP Clients like Claude or Copilot that operates on behalf of users.

### Microsoft Entra ID as authorization server

#### Step 1: Create an Entra ID app registration and a Flow MCP Server App
1. Create an Entra ID app registration by following [these steps](../workspaces/mcp-server-settings.md#step-1-create-the-app-registration).
2. Create an MCP Server App by following [these steps](../workspaces/mcp-server-settings.md#step-5-update-the-flow-mcp-server-app).

#### Step 2: Enable VS Code sign-in
1. Sign in to the [Azure portal](https://portal.azure.com).
2. Go to **Entra ID** -> **App registrations** and open the app registration you created in step 1.
3. Go to **Expose an API** and click **Add a client application**.
4. Enter `aebc6443-996d-45c2-90f0-388ff96faa56` as Client ID and select all listed scopes. Then click **Add application**. This authorizes VS Code to authenticate with you Entra App Registration.

#### Step 3: Add the Semantic Model MCP service URL
1. In Flow, open a **Semantic Model**.
2. Go to **Model Settings** -> **Connect MCP Client** and copy the url in the **Connect with server specific URL** tab.
3. Open VS Code and press **Ctrl + Shift + p**.
4. Select or enter **MCP: Add server...**
5. Select **HTTP**
6. Paste the url you copied in `step 2` above and complete the remaining steps.
7. You may need to restart VS Code and then start the registered MCP Server if it does not automatically initiate a login session. 
    - Press **Ctrl + Shift + p** -> **MCP: List servers** -> select your MCP Server and select **Start server**

>[!NOTE]
> You may need to turn off the native MSAL broker in VS Code if you are running on Windows.  
> 1. Press **Ctrl + Shift + P** -> **Preferences: Open User Settings (JSON)**  
> 2. Add `"microsoft-authentication.implementation": "msal-no-broker"` to the user settings config