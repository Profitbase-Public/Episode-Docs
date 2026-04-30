# Connection

To connect to Hypergene Portfolios you need to specify your `login credentials` (username + password) and `Portfolio ID`.  
Optionally, you can also specify the `Domain` if you are not hosted in the standard Portfolios cloud environment.

![Portfolios Connection](/images/flow/portfolios-connection.png)

<br/>

## Properties
| Name            | Required | Description                      |
|-----------------|------------------|----------------------------------|
| Portfolio ID    | Yes | The Portfolio ID to connect to.  |
| Domain          | No | Specify the domain if you are not hosted in the standard Portfolios cloud environment. The default value is 'https://hub.portfolio.hypergene.cloud'. |
| User name       | Yes | The user name to authenticate with.    |
| Password        | Yes | The password to authenticate with.     |

<br/>

## Description
If you are already familiar with the Portfolios HTTP API, defining a connection enables Flow to connect to the API v2 endpoint at `https://hub.thinking1.com/{portfolio-id}/api/v2` using the provided credentials for authentication.  
If you specify a custom `Domain`, the address will be `{domain}/{portfolio-id}/api/v2`, for example `https://apps.myorg.com/portfolios/{portfolio-id}/api/v2` where `https://apps.myorg.com/portfolios` is the custom domain. 

Having a connection defined, you can then use the [Get report data](./get-report-data.md) and [Upload data](./upload-data.md) actions to fetch and submit data to Portfolios. 
