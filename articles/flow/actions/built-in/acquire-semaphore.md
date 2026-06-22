# Acquire semaphore

Acquires a semaphore before continuing. The semaphore is a `counting semaphore`, meaning 1 or more consumers can acquire it concurrently.  

A counting semaphore allows up to N consumers in at once, useful for things like limiting how many actions or Flows can perform tasks in a specific scope concurrently. It prevents race conditions by making consumers wait their turn, or throttles the number of concurrent jobs that can run simultaneously.

![img](/images/flow/acquire-semaphore.png)  

**Example** ![Example](/images/strz.jpg)  
This Flow processes data for a range of departments by executing a nested Flow using the [Run Flow](./run-flow.md) action. To ensure that no other Flows are processing data for the same department at the same time, it acquires a semaphore using the DepartmentID as the `semaphore name` and `max slots` as 1. Other Flows in the Workspace must use the same configuration (semaphore name and max slots) to ensure synchronization between separate Flows.

<br/>

## Properites
| Name        | Required | Description |
|------------|----------|-------------|
| Title      | No | The title of the action. |
| Semaphore name | No | The name of the semaphore to acquire. |
| Max slots | No | The maximum number of consumers that can acquire the semaphore concurrently. Default is 5. When this threshold is reached, consumers must wait until the semaphore is released by other consumers. |
| Description | No | A description of the action. |

<br/>

### Using Semaphores to throttle and synchronize concurrent Flow executions
Semaphores limit concurrent actions or Flow executions, acting as a synchronization point. Use them to:  
- Fan out work across parallel Flows and wait for all of them to complete before continuing.
- Throttle or synchronize work within a specific scope — such as a department, database, or business domain — to prevent race conditions or guard against resource exhaustion (memory, network I/O, disk).

Multiple semaphores with different names can coexist in a Workspace, letting you create separate execution groups — such as one per Flow, one pr business area, or ones that span several Flows.

A semaphore has two properties:

- **Name** — identifies the semaphore, scoped to the Workspace. All Flows in the same Workspace share a semaphore with the same name.
- **Max slots** — the maximum number of Flows that can execute concurrently.  

>[!NOTE]
>All Flows in the same Workspace share a semaphore with the same name.

The following rules apply:

- **Name** is scoped to the Workspace — all Flows in the same Workspace share a semaphore with the same name.
- The first action to run creates the semaphore and sets its `max slots`.
- `Max slots` remains fixed until all leases have been released.
- Once all leases are released, the next caller to acquire a lease sets the new `max slots`. 
