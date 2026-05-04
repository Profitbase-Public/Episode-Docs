# Azure Event Hub overview

Flow includes built-in support for sending messages to [Azure Event Hub](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-about), letting flows publish events to a hub that other systems can consume — for example, streaming invoices, transactions, or other records as they are processed.

To send messages, you first need an [Azure Event Hub connection](./connecting-to-azure-event-hub.md) configured with a connection string from your Event Hubs Namespace in the [Azure Portal](https://portal.azure.com).

<br/>

## Explore

#### Connection
Set up the connection used by every Event Hub action. Uses an Event Hubs Namespace connection string copied from a shared access policy in the Azure Portal — for sending, a policy with **Send** permission is sufficient. The connection page also covers security recommendations, such as avoiding the namespace-wide RootManageSharedAccessKey in favor of dedicated policies with minimal permissions.  
[Read more](./connecting-to-azure-event-hub.md)

<br/>

#### Send message
Sends a single message to the Event Hub configured in the connection. Takes the message content as a variable and optionally a partition ID for controlling which partition the event is written to.  
[Read more](./send-message.md)
