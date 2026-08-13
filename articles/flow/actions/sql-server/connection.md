# SQL Server Connection

To use SQL Server actions in **Hypergene Flow**, you need to configure a connection by either selecting an **existing connection** or creating a new one.

<br/>

## Connection Type

Before entering server details, select the appropriate **connection type** (authentication method). Available options:

- **SQL Server Authentication** – Use a SQL Server username and password.
- **Microsoft Entra Password** – Authenticate using a Microsoft Entra ID (formerly Azure AD) username and password - not recomended du to MFA.
- **Microsoft Entra Service Principal** – Authenticate using an Entra service principal with a client ID and secret.
- **User Connection String** – Supply a full custom connection string manually.

<br/>

## SQL Server and Microsoft Entra Password Authentication

| Property                       | Required | Description |
|--------------------------------|----------|-------------|
| Server name                    | Yes | The SQL Server hostname, IP address, or named instance. |
| Database name                  | Yes | The specific database to connect to. |
| Username                       | Yes | The user login or Entra ID (email address). |
| Password                       | Yes | The password for the user. |
| Enable Multiple Active Result Sets | Yes | Allows multiple concurrent queries on a single connection. [Learn more](https://learn.microsoft.com/en-us/sql/connect/ado-net/sql/enable-multiple-active-result-sets?view=sql-server-ver16). |

<br/>

## Microsoft Entra Service Principal Authentication

| Property                       | Required | Description |
|--------------------------------|----------|-------------|
| Server name                    | Yes | The SQL Server hostname, IP address, or named instance. |
| Database name                  | Yes | The specific database to connect to. |
| Client Id                      | Yes | The App Registration (client) ID from Azure AD. |
| Client secret                  | Yes | The secret associated with the App Registration. |
| Enable Multiple Active Result Sets | Yes | Allows multiple concurrent queries on a single connection. [Learn more](https://learn.microsoft.com/en-us/sql/connect/ado-net/sql/enable-multiple-active-result-sets?view=sql-server-ver16). |

<br/>

## User Connection String

| Property         | Required | Description |
|------------------|----------|-------------|
| Connection String | Yes | A full custom connection string that defines all necessary parameters for connecting to SQL Server. |

This method bypasses form-based input and should be used by advanced users who prefer complete control over connection parameters.

<br/>

## Dynamic Connections

A [Dynamic Connection](./create-connection.md) can override a static connection during Flow execution.  
This is useful for scenarios where credentials or targets are determined at runtime (e.g., multi-tenant environments).

<br/>

## Best Practices

- Validate the connection using the **Test Connection** feature before saving.
- Store sensitive values like passwords or secrets securely using protected fields.
- Use **Entra Service Principal** for automated, secure, and role-based integration with SQL Server in Azure.

<br/>

## Related Documentation

- [Enable Multiple Active Result Sets](https://learn.microsoft.com/en-us/sql/connect/ado-net/sql/enable-multiple-active-result-sets?view=sql-server-ver16)
- [Create a Dynamic SQL Server Connection](./create-connection.md)