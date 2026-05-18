# Azure Service Bus Queue trigger

Configures the flow to automatically run when a new message is received from an [Azure Service Bus Queue](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#queues). The trigger is push-based — the Flow runs as soon as a message arrives, processing one message at a time.

![topic](/images/flow/queue-trigger.png)


**Example**![Example](/images/strz.jpg)   
This Flow listens for new customer messages on an Azure Service Bus **queue**.  
For each message, it validates that required customer fields are present and, if valid, inserts the record into the database.
<br/>

## Properties

| Name           | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| Title          | No | A descriptive label for the trigger configuration. |
| Service Bus Connection | Yes | [Azure Service Bus connection](../../actions/azure-service-bus/connecting-to-azure-service-bus.md). |
| Default data   | No | Default input data used if no message data is available; useful for testing the Flow with mock data. |
| Output data type | No | Specifies the format of the trigger's output data (e.g., string, JSON, XML). |
| Output variable name | Yes | Name of the variable where the trigger's output data will be stored. |
| Disabled       | No | Boolean value indicating whether the trigger is disabled (true/false). |
| Description    | No | Additional notes or comments about the trigger's purpose or configuration. |

<br/>

## Returns

This trigger returns a single message stored in the variable with the specified name and Output data type. Each trigger execution processes one message at a time, not a batch.

![Schedule Trigger Output Type](../../../../images/flow/schedule-trigger-output-type.png)
<br/>