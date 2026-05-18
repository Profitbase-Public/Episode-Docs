# Azure Service Bus Topic trigger

When a new message arrives in the [Azure Service Bus topic](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#topics-and-subscriptions) subscription, the trigger detects it and starts the flow to process the message. The trigger is push-based — the Flow runs as soon as a message arrives, processing one message at a time.

![Topic Trigger](/images/flow/topic-trigger.png)


**Example**![Example](/images/strz.jpg)   
This Flow listens for new customer-related messages published to an Azure Service Bus **topic**.  
When a message arrives, the Flow checks whether the payload contains the expected customer information.  
- If the required data exists, the customer record is inserted into the database.  
- If the data is missing, the Flow simply continues without writing to the database (in a production scenario you might also add logging or route invalid messages to a dead-letter queue).

<br/>

## Properties

| Name           | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| Title          | No  | A descriptive label for the trigger configuration. |
| Service Bus Connection | Yes | [Azure Service Bus connection](../../actions/azure-service-bus/connecting-to-azure-service-bus.md). |
| Subscription name | No  | Name of the subscription under the Azure Service Bus topic to monitor for new messages. If left empty, Flow will automatically create a subscription. |
| Default data   | No  | Default input data used if no message data is available; useful for testing the Flow with mock data. |
| Output data type | No  | Specifies the format of the trigger's output data (e.g., string, JSON, XML). |
| Output variable name | Yes | Name of the variable where the trigger's output data will be stored. |
| Disabled       | No  | Boolean value indicating whether the trigger is disabled (true/false). |
| Description    | No  | Additional notes or comments about the trigger's purpose or configuration. |

<br/>

## Returns

This trigger returns a single message stored in the variable with the specified name and Output data type. Each trigger execution processes one message at a time, not a batch.

![Schedule Trigger Output Type](../../../../images/flow/schedule-trigger-output-type.png)
<br/>


**Example**![Example](../../../../images/strz2.jpg)   
This flow listens for messages from an Azure Service Bus topic, deserializes them into a ``MessageObject``, and stores the data in a SQL Server table. If the table or necessary columns don't exist, they are created or modified accordingly.

![Topic Trigger2](https://profitbasedocs.blob.core.windows.net/flowimages/topic-trigger2.png)