# Multi-Trigger

Allows creating a trigger that is called from different sources. The sources are: HTTP, Flow and Schedule.

<br/>

![Multi Trigger](/images/flow/multi-trigger.png)

**Example**![Example](/images/strz.jpg)  
This flow can be triggered from multiple sources (HTTP, another flow, or a scheduled event) and processes customer data depending on the trigger type. When triggered, it evaluates the source:

- [If](../actions/built-in/if-else.md) the trigger is an HTTP request, the flow inserts new customers into the database.

- If the trigger is from another flow, it validates the customer records.

- If the trigger is a scheduled event, it updates existing customer data.

- For all other cases, the flow throws an exception with a custom error message.

<br/>

## Properties

| Name           | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| Title          |  No       | A descriptive label for the trigger configuration.|
| Trigger Configurations     | Yes      | Specifies the configurations included in the trigger. |
| Ouput variable name | No       | Name of the variable where the trigger's output data will be stored. |
| Disabled			| No       | Boolean value indicating whether the trigger is disabled (true/false). |
| Description    | No       | Additional notes or comments about the trigger's purpose or configuration. |

<br/>

## Returns

The return type is TriggerOutput, which has the following properties:

| Name           | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| TriggerName    |  String  | Specifies the name of the trigger configuration that triggered the current run. |
| TriggerData    |  Object  | Specifies the data related to the trigger configuration. |

