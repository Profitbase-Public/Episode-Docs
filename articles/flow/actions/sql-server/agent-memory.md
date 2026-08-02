# AI chat history memory

Stores and retrieves the conversation history of an AI agent session, using SQL Server as backing store.

To support ongoing conversations instead of starting from scratch on each request, the AI agent must be configured with memory. Memory allows the agent to retain previous messages and reuse that context when handling new requests. This enables consistent responses, reduces repetition, and preserves conversational continuity across interactions.

As a result, the agent can maintain context over time and respond in a more coherent and predictable way.

![SQL Server Agent Memory](/images/flow/sql-server-agent-memory.png)

**Example** ![Example](../../../../images/strz.jpg)  
This example shows an agent using 3 [Flow AI tools](../ai/flow-ai-tool.md) for working with salary information and calculating a forecast. The Agent memory tracks the conversation history, allowing users to have an ongoing conversation with the agent.

<br/>

## Properties
| Name               | Required | Description                  |
|--------------------|---------------|------------------------------|
| Connection         | Yes | The [SQL Server Connection](./connection.md). |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| Table              | Yes | The name of the table where the memory is stored. If the table does not exist in the database, it will be created automatically by Flow. If you want to use an existing table, see the [description below](#agent-memory-table-schema). |
| Command timeout (sec) | No | The time limit for command execution before it times out. Default is 120 seconds. |

<br/>

## Returns
The action returns the conversation history to the AI agent, based on its Session ID.

<br/>

## Agent memory table schema

If you want to use an existing table to store the agent memory, it must have the following schema.

| Column name   | Data type         | Primary key | Nullable  |
|---------------|-------------------|-------------|-----------|
| SessionID     | varchar(36)       | No          | No        |
| Memory        | nvarchar(MAX)     | No          | Yes       |
| TS            | datetimeoffset(2) | No          | Yes       |


```sql
CREATE TABLE YOUR_TABLE_NAME (
    SessionId VARCHAR(36) NOT NULL,
    Memory NVARCHAR(MAX) NULL,
    TS DATETIMEOFFSET(2) DEFAULT GETUTCDATE() NULL
)
```
