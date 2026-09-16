# Create Monitor ERP connection


This action creates a connection for Monitor ERP and is intended for dynamically connecting to different customers based on parameters or conditions during the execution of a Flow.   

A *Dynamic Connection* overrides the *Connection* during Flow execution.

If you store the credentials for Monitor ERP outside Flow (for example in your own Azure SQL or PostgreSQL database), use this action to _dynamically_ create a connection. The connection returned from the action must then be used as the input to the `Dynamic connection` property of a Monitor ERP request action.

<br/>

![Monitor ERP Create Connection](/images/flow/monitor-erp-create-connection.png)

<br/>

##  Properties

| Name         | Required | Description                                            |
|--------------|----------|--------------------------------------------------------|
| [Host](https://api.monitor.se/articles/v1/url.html) | Yes | The **Host** part of the URL is the address of the Monitor ERP server that you are targeting. |
| [Language code](https://api.monitor.se/articles/v1/url.html) | Yes | The language code specifies the language that the API will scope the request to. It is case-insensitive and must be a valid ISO-639-1 (two letter) language code (default: sv). |
| [Company number](https://api.monitor.se/articles/v1/url.html) | Yes | A company number is the compound of a database number and company id. The company id is currently always 1. |
| Username | Yes | Authentication against the API is performed using the credentials of a normal Monitor ERP user. |
| Password | Yes | Corresponding password for the user. |