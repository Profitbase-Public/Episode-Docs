# Connecting to Stratsys

When adding a Stratsys connection, select an **existing connection** or create a new one.

Stratsys supports two authentication modes: **OAuth client credentials** and **SCIM bearer token**. Flow handles authentication automatically — for OAuth, it exchanges the Client ID and Client Secret for an access token and refreshes it as needed, so you do not need to manage tokens yourself.

<br/>

## Authentication modes

| Mode | Use for |
|------|---------|
| OAuth client credentials | The Stratsys KPI API and the REST API endpoints, such as KPIs, departments, users, scorecards, nodes, and due diligence. This is the default. |
| SCIM bearer token | The SCIM endpoints (`v1/scim/...`), such as SCIM users and groups, used with [REST API Request](./rest-api-request.md) and [REST API Request with paging](./paged-rest-api-request.md). |

The properties you fill in depend on the selected mode.

<br/>

## Connection properties

| Name            | Applies to | Description    |
|-----------------|------------|----------------|
| Name            | Both | Name of the connection object. |
| Authentication mode | Both | **OAuth client credentials** (default) or **SCIM bearer token**. |
| Tenant ID       | OAuth | The Stratsys tenant ID. |
| Company code    | Both | The Stratsys company code. It determines the API address (`https://<company code>.api.stratsys.com`) that Flow sends requests to. It can only contain letters, digits, and hyphens. |
| Client ID       | OAuth | The API client ID. |
| Client secret   | OAuth | The API client secret. |
| SCIM key        | SCIM | The pre-generated SCIM key used as a bearer token. |
| Default external source | Both | The default source system, as defined in Stratsys, to tag data with. Type the value directly into this field. Can be overridden per action on [Push data to Stratsys KPI API](./push-data-to-kpi-api.md), and per request with the `Stratsys-ExternalSource` header on the [REST API Request](./rest-api-request.md) actions. An external source is required by [Push data to Stratsys KPI API](./push-data-to-kpi-api.md) and the `v1/external/...` endpoints, so set it here unless you always set it on the action or request. |

Use the **Test connection** button to verify that the credentials work. For SCIM connections, the test calls the SCIM users endpoint, so the key must have access to it.

<br/>

## Creating Client ID with Secret

You generate the Client ID and Client Secret in Stratsys. See the [Stratsys documentation](https://www.stratsys.com/) for how to create API credentials.

<br/>

## Creating a SCIM key

You generate the SCIM key in Stratsys. See the [Stratsys documentation](https://www.stratsys.com/) for how to generate a SCIM key. The key must not contain whitespace. Store the key in a secret workspace variable when you use it in [Create Stratsys connection](./create-connection.md).

<br/>

> [!NOTE]
> A [Dynamic Connection](./create-connection.md) can replace the default connection described here.
