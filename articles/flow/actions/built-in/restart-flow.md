# Restart Flow

Restarts the Flow at a specified time.

The Restart Flow action is typically used when you need to stop a Flow's execution and continue at a later time. If you don't know in advance when to resume, you can't use a [Schedule trigger](../../triggers/schedule-trigger.md).

For example, if you are interfacing with a rate-limited service and have reached its maximum quota, you need to stop the current execution and wait until the quota resets.

The pause duration varies by service, and wait times of up to an hour are not uncommon. Because a Flow may be terminated if it runs for too long, use the Restart Flow action to end the current execution and resume work later.

Before the Restart Flow action, store the current state using a data storage action, and design the Flow to load that state at the beginning of each run.

![stfl](../../../../images/flow/restart-flow.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow retrieves financial data from XLedger, stores it in a database, and checks if there are remaining credits. If credits are available, it continues fetching data. If the 50,000 credit limit is reached, the flow restarts after a 60-minute delay to resume processing.




## Properties 

| Name        | Required | Description |
|------------|----------|-------------|
| Title      | No | The title of the action. |
| Next run at | Yes | The scheduled date and time (DateTimeOffset) when the flow will execute next. |
| Input      | No | The input data passed to the flow. |
| Description | No | A description of the flow. |

<br/>

### Input (payload)

The optional input is sent as a payload to the Flow when it restarts. It must be a [built-in C# value type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/built-in-types), a string, or a byte array. Objects and records must be serialized, for example, using the [Convert](./convert.md) action.
