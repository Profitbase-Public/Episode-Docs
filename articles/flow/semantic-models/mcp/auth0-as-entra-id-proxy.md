# Auth0 as Entra ID proxy for MCP servers

Nov 18, 2026 · @Tore Senneseth

## Overview

Auth0 acts as the OAuth authorization server for your MCP server, and Entra ID is only the login behind it. MCP clients (Claude, VS Code) talk to Auth0, which handles client registration and the `resource` parameter that Entra can't, then sends the user to Entra to sign in.

**Before you start, have these ready:**

- Auth0 tenant domain, e.g. `<tenant>.us.auth0.com` (called `<auth0-domain>` below).
- The exact MCP server URL users will enter in the client. It becomes the Auth0 API identifier and the token audience.
- Admin access to Auth0 and to the Entra tenant used for sign-in.
- Note: the free Auth0 plan allows one enterprise connection, which is enough for this setup.

## Step 1: Register the app in Entra ID

Create an Entra app registration that Auth0 will use to sign users in.

1. Go to **Microsoft Entra admin center → Entra ID → App registrations → New registration**.
2. **Name:** e.g. `MCP Login (Auth0)`.
3. **Supported account types:** *Single tenant only* to start with your own tenant, or *Accounts in any organizational directory (Multitenant)* if customers from other tenants will sign in.
4. **Redirect URI:** platform **Web**, value `https://<auth0-domain>/login/callback`. If you later use an Auth0 custom domain, add `https://<custom-domain>/login/callback` as well.
5. Click **Register** and copy the **Application (client) ID** and **Directory (tenant) ID**.
6. Go to **Certificates & secrets → New client secret**, then copy the secret **Value** immediately (it is shown once).
7. Under **API permissions**, the default Microsoft Graph `User.Read` is enough for sign-in. Grant admin consent if your tenant requires it.
8. Optional: under **Token configuration → Add optional claim → ID**, add `preferred_username`/`upn` if you want them available to the Action in Step 6.

## Step 2: Add Entra ID as a connection in Auth0

1. In the Auth0 Dashboard go to **Authentication → Enterprise → Microsoft Azure AD → Create Connection**.
2. Fill in:
   - **Connection name:** e.g. `entra-mcp` (this name also appears in the user's Auth0 ID, `waad|<connection name>|<oid>`).
   - **Microsoft Azure AD Domain:** your tenant domain, e.g. `contoso.onmicrosoft.com`. For multi-tenant sign-in, check Auth0's current Azure AD connection docs for the multi-tenant option.
   - **Client ID** and **Client Secret:** from Step 1.
   - **Identity API:** Microsoft Identity Platform (v2.0).
3. Click **Create**.
4. Use the connection's **Try** button to confirm that an Entra login works before continuing.

What to expect afterwards: Auth0 stores the Entra user as `waad|<connection>|<oid>` and keeps `oid` and `tenantid` on the profile. Only a fixed set of Entra attributes is stored, so a claim such as `preferred_username` may be missing (see Step 6).

## Step 3: Turn on the MCP tenant settings

These tenant toggles let MCP clients register and let Auth0 understand the standard `resource` parameter. Go to **Settings → Advanced** and enable:

- **Resource Parameter Compatibility Profile:** Auth0 uses `resource` as the audience when a request has no `audience`. Do not set a tenant-wide Default Audience instead, because that changes behavior for your other applications.
- **Include Issuer in Authorization Responses:** adds the `iss` parameter to authorization responses.
- **Client ID Metadata Document (CIMD) Registration:** lets clients such as Claude and VS Code identify themselves with a URL.
- **Dynamic Client Registration (DCR):** keep the security mode on **Strict**. Some dashboards label it "OIDC Dynamic Application Registration". Keep it on as a fallback for clients that don't support CIMD.
- **Enable Application Connections:** new applications, including the ones MCP clients register, get access to your existing connections.

Click **Save** at the bottom of the page.

## Step 4: Promote the connection to domain level and show it as a button

Both settings are on the Entra connection, and the login fails without them.

**Promote to domain level** (required because MCP clients are registered as third-party applications, which can only use domain-level connections):

1. Go to **Authentication → Enterprise → Microsoft Azure AD →** your connection **→ Settings** tab.
2. Turn on **Promote Connection to Domain Level**.
3. Click **Save**. No CLI is needed.

**Display the connection as a button** (required for the browser login flow to start):

1. On the same connection, open the **Login Experience** tab.
2. Turn on **Display connection as a button**.
3. Click **Save**.

Why the button matters: by default Auth0's login page uses *Identifier + Password* and hides enterprise connections. With no database connection and no button, the client gets `no connections enabled for the client` and the log shows an empty `connection_id`.

## Step 5: Create the API that represents your MCP server

The API's identifier is what MCP clients send as `resource`, and Auth0 puts it in the token's `aud` claim, so it must equal your MCP URL exactly.

1. Go to **Applications → APIs → Create API**.
2. **Name:** e.g. `MCP Server`.
3. **Identifier:** the exact MCP URL users enter in the client, including path, e.g. `https://mcp.example.com/mcp`.
4. **Signing algorithm:** RS256. Click **Create**.
5. Open the API's **Permissions** tab and add the permission your MCP server advertises, e.g. `user_impersonation`. Auth0 only puts scopes in a token when the API defines them, so a missing permission shows up as a missing scope.
6. Open the **Settings** tab, scroll to **Application Access Policy**, and set **User-Delegated Access** to **Per-app authorization**. Click **Save**.
7. Optional on the same tab: turn on **Allow Offline Access** if clients should get refresh tokens (VS Code and Claude request `offline_access`).

Per-app authorization means each MCP client application needs its own grant on this API. You add those grants in Steps 7 and 8, after the client applications exist.

One limit to plan for: the identifier must match the `resource` exactly, so every distinct MCP URL needs its own Auth0 API. Keep the number of distinct URLs small, for example one per MCP server type instead of one per tenant, workspace or model.

## Step 6: Add a post-login Action to set the user claim

The Action copies the user's UPN (or email) into the access token as a namespaced claim your MCP server can read. Auth0 ignores custom claims whose names aren't URLs, which is why the claim name is a full URL.

1. Go to **Actions → Library → Create Action → Build from scratch**.
2. **Name:** e.g. `Add nameidentifier claim`. **Trigger:** **Login / Post Login**. Click **Create**.
3. Paste this code:

```typescript
exports.onExecutePostLogin = async (event, api) => {
  let upn = event.user.identities?.[0]?.profileData?.preferred_username;
  if (!upn) {
    upn = event.user.email;
  }    
  api.accessToken.setCustomClaim("https://schemas.hypergene.com/identity/claims/nameidentifier", upn);
};
```

4. Click **Deploy**.
5. Go to **Actions → Triggers → post-login**, drag your Action from the custom list into the flow between **Start** and **Complete**, and click **Apply**. A deployed Action that isn't in the flow does nothing.

Notes:

- `preferred_username` is only present if Auth0 stored it from Entra, so the code falls back to the email address.
- The claim appears on newly issued access tokens only. Disconnect and reconnect in the MCP client to get a fresh token.
- To debug, open the Action and use **Real-time Logs** while logging in, or decode the access token and look for the claim.
- For identity decisions, prefer the stable `oid` and `tenantid` on the Auth0 profile over the email, because `email_verified` is `false` for Entra users.

## Step 7: Add Claude as an application

Auth0 doesn't register a CIMD client the first time it signs in, so each client must be imported once. Claude's metadata URL (its client ID) for claude.ai web, Desktop and mobile is `https://claude.ai/oauth/mcp-oauth-client-metadata`.

1. Go to **Applications → Applications → Create Application**.
2. Choose **Import from URL**.
3. Enter `https://claude.ai/oauth/mcp-oauth-client-metadata` and click **Preview**. Check the name and redirect URI shown.
4. Click **Create**. The redirect URI (`https://claude.ai/api/mcp/auth_callback`) comes from the metadata document, so there's nothing to enter.
5. Grant the app access to your API: go to **Applications → APIs →** your MCP API **→ Application Access** tab, click **Edit**, find the Claude application, select **Grant Access** under **User-Delegated Access**, choose the permissions (e.g. `user_impersonation`), and save.
6. Check the new application's **Connections** tab: the Entra connection should be available. If it isn't, re-check Step 3 (Enable Application Connections) and Step 4.

Claude Code is a separate client with its own metadata URL (`https://claude.ai/oauth/claude-code-client-metadata`). Import it the same way if you want to support it.

## Step 8: Add VS Code as an application

VS Code identifies itself with the metadata URL `https://vscode.dev/oauth/client-metadata.json`. Without importing it, the login fails with `Unknown client`.

1. Go to **Applications → Applications → Create Application**.
2. Choose **Import from URL**.
3. Enter `https://vscode.dev/oauth/client-metadata.json` and click **Preview**.
4. Click **Create**. VS Code's loopback redirect URIs come from the metadata document.
5. Grant the app access to your API: **Applications → APIs →** your MCP API **→ Application Access** tab → **Edit** → find the VS Code application → **Grant Access** under **User-Delegated Access** → choose the permissions (e.g. `user_impersonation`) → save. Without this grant the login fails with `Client ... is not authorized to access resource server`.
6. Check the application's **Connections** tab for the Entra connection, as in Step 7.

Repeat Steps 7 and 8 for every other MCP client you want to support, each with its own metadata URL and its own grant.


## Step 9: Test and troubleshoot

To test, add the MCP server URL as a custom connector in Claude (Settings → Connectors), or add it as an MCP server in VS Code. You should get the Entra sign-in, then a consent screen, and the connection should succeed. For any failure, open **Monitoring → Logs** in Auth0 and read the failed entry.

| Error or symptom | Cause | Fix |
| --- | --- | --- |
| `invalid_request: Unknown client: <metadata URL>` | The client was never imported | Import it via Create Application → Import from URL (Steps 7 and 8) |
| `Client "tpc_..." is not authorized to access resource server` | No grant for that client on the API | API → Application Access → Edit → Grant Access (Step 5) |
| `no connections enabled for the client`, empty `connection_id` in the log | Connection isn't domain-level, or is hidden from the login page | Promote Connection to Domain Level and Display connection as a button (Step 4); check Enable Application Connections (Step 3) |
| Token has no `user_impersonation` scope | Permission not defined on the API, or no grant for the client | Add it under the API's Permissions tab and grant it to the client (Steps 5, 7, 8) |
| Custom claim missing from the token | Action not deployed or not in the post-login flow, claim name isn't a URL, or a stale token | Deploy and apply the Action (Step 6), then disconnect and reconnect the client |
| Entra error about the redirect URI | Redirect URI mismatch | Entra app must list `https://<auth0-domain>/login/callback` exactly (Step 1) |
| Claude connects with DCR instead of CIMD | Auth0 metadata lacks the CIMD flags Claude requires | Check `openid-configuration` (Step 9); DCR works but registers a client per connection |

Source: [Authentication for connectors, Claude documentation](https://claude.com/docs/connectors/building/authentication.md) (Claude's OAuth requirements: 401 handling, CIMD conditions, callback URLs, egress range).
