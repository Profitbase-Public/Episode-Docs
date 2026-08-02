# AI agent file access

Enables AI agents to access a virtual file system to create, read, write, search and delete files and folders. The action uses a SQL Server table to emulate a disk drive, meaning all data is read and written to SQL Server.

Typical use cases for this node:  
- Enable the [Harness AI agent](../agents/harness-ai-agent.md) to store its result in one or more text files (for example JSON) so it can be retrieved for further processing (e.g using another SQL Server action)
- Provide input data (context) to AI agents (like the [Harness AI agent](../agents/harness-ai-agent.md)) through a file system API. You can do this by inserting rows to the table ([described below](#manually-reading-and-writing-data-to-the-file-access-table)) before running the agent. The agent will then be able to access the files as resources and use them as input while working.
- Enable AI agents to use a file system API to store temporary files or data it's working with, enabling it to offload data from its context window.


![img](/images/flow/sql-server-agent-file-access.png)
**Example** ![Example](../../../../images/strz.jpg) The example above shows a Flow with a [Harness AI agent](../agents/harness-ai-agent.md) using the `AI agent file access node for SQL Server` (described in this document) to store its output so it can be retrieved using the [Load to DataTable](./load-to-datatable.md) action when the agent has finished its work. The agent has instructions to store its output in a JSON file, and will use the `AI agent file access` node to do so.

<br/>


## Properties
| Name               | Required | Description                  |
|--------------------|---------------|------------------------------|
| Connection         | Yes | The [SQL Server Connection](./connection.md). |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| Table              | Yes | The name of the table where the to use as file storage. If the table does not exist in the database, it will be created automatically by Flow. If you want to use an existing table, see the [description below](#table-definition). |
| Command timeout (sec) | No | The time limit for SQL command execution before it times out. Default is 120 seconds. |

<br/>

## How to use the node
To use this node, attach it to the `Context providers` port of an AI agent action (e.g the [Harness AI agent](../agents/harness-ai-agent.md)) like shown in the example above.  
Then go its `Properties` editor and specify the `Table` name. 

When setting up the node and specifying the `Table` property, you **DO NOT** have to select and existing table. You can simply provide a name, and the system will automatically create the table if it does not already exist.  

However, if you _want_ to use an existing table, please refer to the [Table definition description](#table-definition) below.

### Manually reading and writing data to the file access table
Consider the file table (defined by [the table definition below](#table-definition)) as a file system for reading and writing files. It provides a way to exchange data with an AI agent using familiar file system concepts. You can add files as input to the agent using SQL INSERT or UPDATE statements, retrieve agent outputs using SELECT, and remove files using DELETE. File names and directory structures are typically defined as part of the agent’s instructions.

**Examples**  

```sql
-- Add file to the root folder. Note that EntryType = 1 (File).
INSERT INTO my_agent_files_table (Directory, EntryName, EntryType, Content) VALUES('/root', 'actuals.csv', 1, 'csv file contents')

-- Create directory 'MyDir' under root. Note that EntryType = 0 (Directory).
INSERT INTO my_agent_files_table (Directory, EntryName, EntryType) VALUES('/root', 'MyDir', 0)

-- Add file to the /root/MyFile folder. Note that EntryType = 1 (File).
INSERT INTO my_agent_files_table (Directory, EntryName, EntryType, Content) VALUES('/root/MyDir', 'customers.json', 1, 'json file data')

-- Fetch the result of an AI agent 
SELECT TOP 1 Content FROM my_agent_files WHERE Directory = '/root/MyDir' AND EntryName = 'variance_analysis.csv'
```

> [!IMPORTANT]
> When adding entries to the files table, `Directory` must always start with `/root`. Otherwise, the AI agent will not be able to locate the file.

<br/>

## Table definition

When setting up the node and specifying the `Table` property, you **DO NOT** have to create the table upfront. You can simply provide the name, and the system will automatically create the table if it does not already exist.  

However, if you want to use an existing table, it **must** have the exact table schema as described below.  
Replace `my_agent_files_table` with your name.

```sql
CREATE TABLE [my_agent_files_table](	
	[Directory] [nvarchar](600) NOT NULL,
	[EntryName] [nvarchar](232) NOT NULL,
	[EntryType] [int] NOT NULL, -- 0 = Directory, 1 = File
	[Content] [nvarchar](max) NULL,
	[Metadata] [nvarchar](1000) NULL,
	[TS] [datetimeoffset](2) NULL,
 CONSTRAINT [PK_my_agent_files_table] PRIMARY KEY NONCLUSTERED 
(	
	[Directory] ASC,
	[EntryName] ASC
)WITH (STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```