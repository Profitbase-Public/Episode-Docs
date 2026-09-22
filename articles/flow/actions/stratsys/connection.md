# Connecting to Stratsys

When adding a Stratsys connection, select an **existing connection** or create a new one.

Stratsys authenticates using OAuth 2.0 client credentials. Flow exchanges the Client ID and Client Secret for an access token automatically and refreshes it as needed — you do not need to manage tokens yourself.

<br/>

## Connection properties

| Name            | Description    |
|-----------------|----------------|
| Name            | Name of the connection object. |
| Tenant ID       | The Stratsys tenant ID. |
| Company code    | The Stratsys company code. |
| Client ID       | The API client ID. |
| Client secret   | The API client secret. |
| External source | The default source system, as defined in Stratsys, to tag pushed data with. Type the value directly into this field. Can be overridden per action on [Push data to Stratsys KPI API](./push-kpi.md). |

<br/>

## Creating Client ID with Secret

You generate the Client ID and Client Secret in Stratsys. See the [Stratsys documentation](https://www.stratsys.com/) for how to create API credentials.

<br/>

> [!NOTE]
> A [Dynamic Connection](./create-connection.md) can replace the default connection described here.
