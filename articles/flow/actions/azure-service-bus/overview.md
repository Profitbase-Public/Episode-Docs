# Azure Service Bus overview

Flow includes built-in support for sending messages to [Azure Service Bus](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview), letting flows publish messages to a [queue](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#queues) or [topic](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#topics-and-subscriptions) for other systems to consume — for example, posting overdue payment records as JSON messages for downstream processing.

To send messages, you first need an [Azure Service Bus connection](./connecting-to-azure-service-bus.md) configured with a connection string from your Service Bus namespace in the [Azure Portal](https://portal.azure.com).

<br/>

## Explore

#### Connection
Set up the connection used by every Service Bus action. Uses a Service Bus namespace connection string copied from a shared access policy in the Azure Portal — for sending, a policy with **Send** permission is sufficient. The connection page recommends using a least-privilege policy (such as Send-only) instead of the root namespace policy.  
[Read more](./connecting-to-azure-service-bus.md)

<br/>

#### Send message
Sends a single message to a queue or topic specified by **Send to**. In addition to the message content and its MIME content type, the action supports optional metadata — Subject, Message Id, Correlation Id, and Reply to — useful for routing, deduplication, and request/reply patterns on the consumer side.  
[Read more](./send-message.md)
