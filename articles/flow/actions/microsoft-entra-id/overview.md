# Microsoft Entra ID overview

Flow includes built-in support for working with [Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/) (formerly Azure Active Directory), letting you automate user lifecycle tasks — inviting guests, creating new users, looking up existing ones — and monitor app registrations, including upcoming client secret expirations.

To use any Entra ID action, you first need an [Entra ID connection](./connecting-to-entra-id.md) authenticated with a Microsoft Entra ID App (Service Principal) configured with Tenant ID, Client ID, and Client Secret. Each action requires specific Microsoft Graph application permissions on that Service Principal — the connection page lists the minimum permission needed for every supported action so you can apply least-privilege.

<br/>

## Explore

#### Connection
Set up the connection used by every Entra ID action. Requires a Microsoft Entra ID App with the appropriate **Application** permissions granted on Microsoft Graph (such as `User.ReadWrite.All` for creating users, or `Application.Read.All` for reading app registrations) and admin consent.  
[Read more](./connecting-to-entra-id.md)

<br/>

#### Managing users
Four actions cover the user-related operations. [Invite guest user](./invite-guest-user.md) sends an invitation to an external email address, with a configurable redirect URL and the option to either send the default invitation email, suppress it, or customize the message — useful when you want to deliver the invitation through a different channel such as [SendGrid](../sendgrid/overview.md). [Create user](./create-user.md) creates a regular tenant user with UPN, optional password (auto-generated if left blank), display name, and other profile fields. [For each user](./for-each-user.md) iterates over all users in the tenant — useful for synchronization or audit flows. [Get user](./get-user.md) looks up a single user by one or more query parameters (such as email or display name) to retrieve their OID, UPN, or other attributes. Both For each user and Get user support an **Include extended profile** flag that pulls richer directory attributes (department, manager, employeeId, etc.) at the cost of a slower response. Invite guest user and Create user also support **Wait for user propagation**, which pauses the flow until the new user is fully synced before continuing.

<br/>

#### Monitoring app registrations
Two actions support tracking app registrations and their secrets — typically used to catch client secrets that are about to expire before they cause production outages. [For each app registration](./for-each-app-registration.md) iterates over the app registrations in the tenant, with an optional case-sensitive prefix filter. [For each client secret info](./for-each-client-secret-info.md) lists the client secrets of a specific app registration including their end dates. The common pattern is iterating over all app registrations, then iterating over each one's secrets, and sending a notification email when any secret is within a configured warning window (such as 30 days from expiry).
