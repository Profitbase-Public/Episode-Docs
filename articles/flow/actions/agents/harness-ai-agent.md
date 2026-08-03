# Harness AI agent

Defines an AI Agent that can perform long-running, complex, multi-step tasks using tools, skills, and external services such as file systems and APIs.

![img](/images/flow/harness-ai-agent.png)

<br/>

## Properties

| Name            | Required | Description                         |
|-----------------|--------------|-------------------------------------|
| Instructions    | Yes | The instruction given at the start of a conversation that sets the agent's behavior, tone, and goals. It defines the role the agent should play (e.g., teacher, assistant, coder), how it should respond (e.g., formally or casually), and optionally the preferred strategy for decision-making.  |
| User prompt     | Yes | This is the actual task that the agent is given, for example "Summarize all Word documents in my OneDrive folder named 'Work', and send the summarization to my-email@corp.com."  |
| Session ID      | No | A session ID is required if you want to enable memory, allowing for an ongoing conversation with the AI, rather than every interaction starting fresh.|
| AI contents     | No | |
| AI model      | Yes | The chat model accepts a set of instructions, reasons about _how_ to solve the task, and uses tools to achieve the final outcome. |
| [Memory](#memory)          | No | Agent memory used to store and retrieve the conversation history with the agent, and to enable the agent to manage internal state (working memory). Allows for an ongoing conversation with the AI, rather than every interaction starting fresh. Note that memory requires a Session ID.  |
| [Tools](#tools)           | No | One or more tools that the agent can use to perform the tasks identified by the chat model. Tools the agent can use includes: <br/> [Azure Blob Storage Agent Tool](../azure-blob-storage/agent-tool.md) <br/> [OneDrive Agent Tool](../onedrive/agent-tool.md) <br/> [Outlook Agent Tool](../microsoft-365-outlook/agent-tool.md) <br/> [Markdown Agent Tool](../markdown/agent-tool.md)<br/>[MCP client tool](../mcp/mcp-client-tool.md) <br/> [Flow AI tool](../ai/flow-ai-tool.md)  | 
| [Skills](#skills)          | No   | One or more [AI agent skills](./ai-agent-skill.md) teaching the agent specialized knowledge and workflows. Skills are also recommended as a way to progressively load tools _on demand_ to reduce token usage and cost. |
| Context providers | No | Services that provide additional context or on-demand capabilities to the AI agent. Use this property to enable virtual file system access for the agent, providing a way for the agent to read additional input data and write its result to files. |

<br/>

## Returns
See [Output summary - final response](#output-summary---final-response)

<br/>

## Providing (input) data and additional context to the agent
In addition to the `User prompt`, `AI contents`, `Tools` and `Skills`, you can also use the `Context providers` property to give the AI agent access to a virtual file system. Suppose you have a CSV file you want the agent to analyse, you can add the file to the file system manually (using a compatible Flow node) and then instruct the agent to read the file(s) as input.  

Please refer to [Reading and writing data to the file access table](../sql-server/agent-file-access.md#manually-reading-and-writing-data-to-the-file-access-table) for a detailed explanation of how this can be done.


<br/>

## Agent output
- [Retrieving the generated output of the agent](#retrieving-the-generated-output-of-the-agent)
- [Output summary - final response](#output-summary---final-response)

### Retrieving the generated output of the agent
The `Harness AI agent` is not designed to return a _result object_ like a file or business object of the task. (With the right instructions, you _can_ do this, but if this is the goal, you should use the [Tools AI agent](./tools-ai-agent.md) instead).  

By providing access to a virtual file system, you enable the agent to store its output as a file (for example JSON or CSV) so it can be retrieved by downstream nodes.   
To enable file output, do the following:
- Specify in the agent's `Instructions` that you want the output written to a file.
- Attach a `file access` node like [AI agent file access for SQL Server](../sql-server/agent-file-access.md) to the `Context providers` property port.

To tell the agent to output the result to file, its instructions may contain something like:
```txt
# Output
- Save the result to a CSV file named variance_analysis.csv in the Outputs folder.
- Ensure that the folder exists
```

To retrieve the output from a `Harness AI agent` in downstream nodes, simply use a node compatible with the `file access` system node used, for any SQL Server action for reading data if you've used [AI agent file access for SQL Server](../sql-server/agent-file-access.md) as a `Context provider`.

**Example**  
Retrieving the output of an AI agent from a SQL Server file access store.  
```sql
SELECT TOP 1 Content FROM my_agent_files WHERE Directory = '/root/Outputs' AND EntryName = 'variance_analysis.csv'
```

Please refer to [Reading and writing data to the file access table](../sql-server/agent-file-access.md#manually-reading-and-writing-data-to-the-file-access-table) for a detailed explanation of how this can be done.

<br/>

### Output summary - final response
When the agent completes its job, it returns a summary object containing the conversation thread and token usage.  
This can be used to review its reasoning and workflow. The token count is useful if you want to implement throttling or record token usage to enforce limits, and also while developing the agent to optimize token usage (creating instructions, selecting tools, etc.)
```json
{
    "content": string,
    "totalTokenCount": int,
    "outputTokenCount": int,
    "inputTokenCount": int,
    "reasoningTokenCount": int
}
```

<br/>

## Memory
The Harness AI agent supports two types of memory;
- Conversation history memory (chat history) with the user which enables an ongoing conversation
- Internal working memory (file memory) for storing and retrieving plans, notes, temporary files, todos, etc as it progresses towards its goal

When building agents that solves complex, long running tasks while interacting with a user, you often need both types of memory.  
To add memory to the agent, attach nodes such as the [AI chat history memory for SQL Server](../sql-server/agent-memory.md) and [AI agent file memory for SQL Server](../sql-server/agent-file-memory.md) to the `Memory` property port. The agent will decide on its own when and if to use them.

![img](/images/flow/harness-agent-memory.png)

<br/>

## Tools
[Tools](../ai/flow-ai-tool.md) extend the agent's capabilities by giving it access to external systems and services. Attach one or more tool nodes to the `Tools` property port to make them available to the agent.

<br/>

## Skills
[Skills](./ai-agent-skill.md) teach AI agents specialized knowledge and workflows — typically things specific to your business that the model would not otherwise know.

<br/>

## Harness AI agent vs Tools AI agent
The Harness AI agent described in this document and the [Tools AI agent](./tools-ai-agent.md) serve different purposes:
- The Harness AI agent is designed to perform complex, long-running tasks and output its result (if any) to a backing store through a tool or context provider like the [AI agent file access for SQL Server](../sql-server/agent-file-access.md) node.
- The [Tools AI agent](./tools-ai-agent.md) returns results directly from its output port as a strongly typed object. The `Harness AI agent` writes results to a file for retrieval by downstream nodes, but can produce multiple outputs during a session.
- The [Tools AI agent](./tools-ai-agent.md) can use `Code mode` when invoking its tools.
- The `Harness AI agent` can stream its progress back to the client, while the [Tools AI agent](./tools-ai-agent.md) cannot.
- The [Tools AI agent](./tools-ai-agent.md) is best for smaller, concrete tasks for producing an output as part of a workflow, while the `Harness AI agent` is best for more complex, long-running tasks while interacting with a user.


