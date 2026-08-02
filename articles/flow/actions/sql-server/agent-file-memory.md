# AI agent file memory

Enables AI agents to use file system based memory for notes, plans, session state, and temporary files, using SQL Server as backing store.

> [!NOTE]
> The difference between the `AI agent file memory` node described in this document and the [AI chat history memory](./agent-memory.md) node, is that it stores plans, notes, temporary files and other session state, but **not** the `conversation history` with the user. 

![img](/images/flow/sql-server-agent-file-memory.png)
**Example** ![Example](../../../../images/strz.jpg) The example above shows a Flow with a [Harness AI agent](../agents/harness-ai-agent.md) using the `AI agent file memory` node for SQL Server (described in this document) to store and retrieve working memory data like notes, plans and state during a session. For example, if the agent needs to ask the user for clarifications or additional information during a session, or get confirmation that a user wants to go ahead with a plan, it can store its progress and plan as files and then pick them up and continue where it left off when the user replies.

<br/>

## Properties
| Name               | Required | Description                  |
|--------------------|---------------|------------------------------|
| Connection         | Yes | The [SQL Server Connection](./connection.md). |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| Table              | Yes | The name of the table where the memory (file contents) is stored. If the table does not exist in the database, it will be created automatically by Flow. If you want to use an existing table, see the [description below](#table-definition). |
| Command timeout (sec) | No | The time limit for command execution before it times out. Default is 120 seconds. |

<br/>

## How to use the node
To use this node, attach it to the `Memory` port of an AI agent action like shown in the example above.  
Then go the the `Properties` window and specify the `Table` property. 

When setting up the node and specifying the `Table` property, the table **DOES NOT** have to already exist. You can simply provide a name, and the system will automatically create the table if it does not already exist.  

However, if you _want_ to use an existing table, please refer to the [Table definition description](#table-definition) below.

<br/>

## Table definition

When setting up the node and specifying the `Table` property, you **DO NOT** have to select and existing table. You can simply provide a name, and the system will automatically create the table if it does not already exist.  

However, if you want to use an existing table, it **must** have the exact table schema as described below.  
Replace `my_agent_files_table` with your name.

```sql
CREATE TABLE [my_agent_files_table](
	[SessionId] [varchar](36) NOT NULL,
	[Directory] [nvarchar](600) NOT NULL,
	[EntryName] [nvarchar](232) NOT NULL,
	[EntryType] [int] NOT NULL,
	[Content] [nvarchar](max) NULL,
	[Metadata] [nvarchar](1000) NULL,
	[TS] [datetimeoffset](2) NULL,
 CONSTRAINT [PK_my_agent_files_table] PRIMARY KEY NONCLUSTERED 
(
	[SessionId] ASC,
	[Directory] ASC,
	[EntryName] ASC
)WITH (STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```