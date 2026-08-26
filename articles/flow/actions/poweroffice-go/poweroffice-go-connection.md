# Connecting to PowerOffice Go

To use PowerOffice Go actions in **Hypergene Flow**, you must either select an **existing connection** or create a new one.

<br/>

The connection authorizes Flow to interact with the PowerOffice Go API on your behalf. It requires authentication credentials issued for your registered client application.

<br/>

## Connection properties

A [PowerOffice Go connection](https://developer.poweroffice.net/documentation/authentication) consists of the following fields:

| Name                  | Description |
|-----------------------|-------------|
| Connection Name       | A user-defined name for this connection. |
| Application Key       | A unique identifier for your application registered with PowerOffice Go. |
| Client Key            | A confidential string used for authenticating your client app. |
| Subscription Key      |  Required if accessing APIs that need an additional API key. |
| Use PowerOffice Go test environment  (optional) |  If checked, the connection will use demo endpoints for testing. |

<br/>

<!-- ![PowerOffice Go Connection](../../../../images/flow/powerofficego-connection.png)

<br/> -->

> [!NOTE]
> A [Dynamic Connection](./create-connection.md) can be used to override this default connection during flow execution.  
> This is useful when connecting to different subscriptions programmatically or pulling credentials from external sources at runtime.

<br/>

## Related documentation

- [PowerOffice Go API authentication](https://developer.poweroffice.net/documentation/authentication)
- [Create dynamic PowerOffice Go connection](./create-connection.md)