# Create Stratsys connection

This action creates a connection for Stratsys and is intended for dynamically connecting to different tenants based on parameters or conditions during the execution of a Flow.

<br/>

A 'Dynamic Connection' will override the 'Connection' during flow execution.

If you store the credentials outside Flow (for example, in your own Azure SQL or PostgreSQL database), use this action to *dynamically* create a connection. The connection returned from the action can be used as the input to the `Dynamic connection` property of the [Push data to Stratsys KPI API](./push-data-to-kpi-api.md) action.

<br/>

![Create Stratsys Connection action showing Tenant ID, Company code, Client ID, Client secret, and Default external source properties](/images/flow/stratsys-create-connection.png)

<br/>

## Properties

| Name                    | Required | Description                                                     |
|-------------------------|----------|-----------------------------------------------------------------|
| Tenant ID               | Yes | The Stratsys tenant ID. |
| Company code            | Yes | The Stratsys company code. |
| Client ID               | Yes | The API client ID. |
| Client secret           | Yes | The API client secret. |
| Default external source | No | The default source system, as defined in Stratsys, to tag pushed data with. Type the value directly into this field. Can be overridden per action on [Push data to Stratsys KPI API](./push-data-to-kpi-api.md). |

The connection is returned on the **connection** output port. Set the **Connection variable name** to reference it later — for example, as the **Dynamic connection** of a Stratsys action.

<br/>

## Creating Client ID with Secret

You generate the Client ID and Client Secret in Stratsys. See the [Stratsys documentation](https://www.stratsys.com/) for how to create API credentials.
