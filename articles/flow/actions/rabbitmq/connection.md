# RabbitMQ connection

To publish messages to a [RabbitMQ](https://www.rabbitmq.com/) broker — or to receive them through the [RabbitMQ message trigger](../../triggers/rabbitmq/message-trigger.md) — you need to either select an existing **RabbitMQ connection** or create a new one.

The connection holds the broker address and credentials, and is reused across actions and triggers.

<br/>

## Connection properties

| Name             | Required | Description |
|------------------|----------|-------------|
| Name             | Yes | A custom name for the connection. This will be shown when selecting it in Flow actions and triggers. |
| Host name        | Yes | The hostname or IP address of the RabbitMQ broker. |
| Port             | No | The port number the broker listens on. Leave empty to use the RabbitMQ default (`5672` for non-TLS, `5671` when **Use SSL** is enabled). |
| User name        | No | The user account used to authenticate against the broker. Required by brokers that have authentication enabled (typical for any non-local setup). |
| Password         | No | The password for the specified user. |
| Virtual host     | No | The [virtual host](https://www.rabbitmq.com/docs/vhosts) to connect to. Leave empty to use the broker's default vhost (`/`). |
| Use SSL          | No | Enable to connect to the broker over a TLS-secured channel. |
| SSL server name  | No | The expected name on the broker's TLS certificate. Used for server name verification when **Use SSL** is enabled and the certificate's name doesn't match the host name (for example when connecting through a load balancer). |

<br/>

![RabbitMQ Connection](../../../../images/flow/rabbitmq-connection.PNG)

<br/>

## Configuration steps

1. Provide a **Name** for the connection.
2. Enter the **Host name** of the RabbitMQ broker.
3. (Optional) Set a **Port** — leave empty for the protocol default.
4. Enter the **User name** and **Password** if the broker requires authentication.
5. (Optional) Specify a **Virtual host** if the broker uses one other than the default.
6. (Optional) Tick **Use SSL** to enable TLS, and provide an **SSL server name** if the certificate name doesn't match the host name.
7. Click **Test connection** to verify the setup.
8. If the test is successful, click **Ok** to save the connection.

<br/>

## Notes and best practices

- Ensure the RabbitMQ broker is reachable from your network. If connecting through a firewall, the relevant port must be open (default `5672`, or `5671` for TLS).
- Use TLS (**Use SSL**) whenever the connection crosses a network boundary you don't fully trust.
- Use strong credentials, rotate passwords regularly, and prefer a dedicated user with permissions limited to the queues and exchanges the flow actually needs.
