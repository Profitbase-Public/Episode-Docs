# Await semaphore drained

Waits for all semaphore leases to be released before continuing.  

Use this action to wait for all Flow executions holding a semaphore lease to complete before continuing execution of the Flow.
It makes it easy to create a synchronization point for simultaneous Flow executions.

![img](/images/flow/await-semaphore-drained.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow starts a data pipeline Flow pr department using the [Start Flow](./start-flow.md) action. The action uses a semaphore to limit the number of concurrently started Flows. The `Await semaphore drained` action after the (For each department) loop ensure that all tasks has completed before the `Consolidate all` SQL command runs at the end.

<br/>

### Properties

| Name                      | Required | Description                                                                       |
| ------------------------- | --------- | --------------------------------------------------------------------------------- |
| Title                     | No | The title or name of the action.                                                 |
| Semaphore name            | Yes | The name of the semaphore in the Workspace to await to be fully drained (all leases released). |
| Description               | No | Additional notes or comments about the action or configuration.                   |
