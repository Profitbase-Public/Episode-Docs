# Create NetSuite connection

This action creates a connection for NetSuite and is intended for dynamically connecting to different organizations based on parameters or conditions during the execution of a Flow.   

A *Dynamic Connection* overrides the *Connection* during Flow execution.

If you store the credentials for NetSuite outside Flow (for example in your own Azure SQL or PostgreSQL database), use this action to _dynamically_ create a connection. The connection returned from the action must then be used as the input to the `Dynamic connection` property of a NetSuite request action.

A connection to NetSuite uses **OAuth2** machine to machine (M2M) authentication.

<br/>

![NetSuite Create Connection](/images/flow/netsuite-create-connection.png)

<br/>

##  Properties

| Name              |  Description                                            |
|-------------------|---------------------------------------------------------|
| Account ID        | The NetSuite account identifier for the target environment (your subscriptionr / realm). |
| API consumer key  | The API consumer key used to authenticate requests (from the page where you create M2M integration in NetSuite). |
| Client credentials certificate ID | The ID of the certificate configured for client credentials authentication (from the OAuth 2.0 Client Credential Setup). |
| Private certificate key | The private certificate key in PEM format used to sign authentication requests. |
