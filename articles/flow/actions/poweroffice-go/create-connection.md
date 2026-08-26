# Create PowerOffice Go Connection

This action creates a connection for PowerOffice Go and is intended for dynamically connecting to different subscriptions based on parameters or conditions during the execution of a Flow.

A 'Dynamic Connection' will override the 'Connection' during flow execution.

If you store the credentials for PowerOffice Go outside Flow (for example, in your own Azure SQL or PostgreSQL database), use this action to ||*dynamically* create a connection. The connection returned from the action must then be used as the input to the `Dynamic connection` property of a PowerOffice Go request action.

## Properties

| Name              | Description                                                      |
|-------------------|------------------------------------------------------------------|
| Application Key   | A unique identifier for the client application.                  |
| Client Key        | A confidential key used to authenticate the client.              |
| Subscription Key  | An API key to access specific PowerOffice services.              |
| Use Demo URLs     | Enables testing in a sandbox environment when checked.           |
