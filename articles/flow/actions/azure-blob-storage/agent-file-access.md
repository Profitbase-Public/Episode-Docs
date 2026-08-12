# AI agent file access

Enables AI agents to access an Azure Blob Storage container as a virtual file system. The agent can create, read, write, search, and delete files and folders while working on a task.

Typical use cases for this node:
- Enable the [Harness AI agent](../agents/harness-ai-agent.md) to store results as files, such as JSON or CSV, so they can be retrieved and processed by downstream Flow actions.
- Provide files from Azure Blob Storage as input and context to an AI agent.
- Give an agent temporary storage for plans, intermediate results, or other data that it does not need to keep in its context window.

![AI agent file access connected to a Harness AI agent](/images/flow/sql-server-agent-file-access.png)

**Connection example**  
The example shows a file access node connected to the `Context providers` port of a Harness AI agent. The screenshot uses the SQL Server version of the node; the Azure Blob Storage version connects to the same port and uses an Azure Blob container connection instead of a table.

## Properties

| Name | Required | Description |
|------|----------|-------------|
| Connection | Yes | The [Azure Blob container connection](./azure-blob-container-connection.md) that provides access to the container used as file storage. |
| Dynamic connection | No | A connection created during Flow execution using the [Create Azure Blob container connection](./create-azure-blob-container-connection.md) action. |

The connection must grant the permissions required by the agent's task. For example, an agent that lists, reads, creates, updates, and deletes files needs corresponding permissions for blobs in the container.

<br/>

## How to use the node

Attach the node to the `Context providers` port of an AI agent action, such as the [Harness AI agent](../agents/harness-ai-agent.md), and select the Azure Blob container connection to use.

The connected container is exposed to the agent as a file system under `/root`. Blob names map to file paths beneath this folder. For example, a blob named `Inputs/actuals.csv` is available to the agent as `/root/Inputs/actuals.csv`.

Include the file names and expected behavior in the agent's instructions. For example:

```txt
Read /root/Inputs/actuals.csv and /root/Inputs/budget.csv.
Compare actuals with the budget, then save the result as
/root/Outputs/variance_analysis.csv.
Create the Outputs folder if it does not exist.
```

> [!IMPORTANT]
> File paths provided to the agent must start with `/root`.

<br/>

## Providing input files and retrieving output

Files already stored in the connected container are available to the agent. To add an input file from a Flow, use [Upload Blob](./upload-blob.md) and specify the blob name without the `/root/` prefix. For example, upload `Inputs/actuals.csv` so the agent can access it as `/root/Inputs/actuals.csv`.

After the agent finishes, use an Azure Blob Storage action to retrieve or process its output. For example, use [Read Blob as stream](./read-blob-as-stream.md) or [Read Blob as byte array](./read-blob-as-byte-array.md) with the blob name `Outputs/variance_analysis.csv`.

Azure Blob Storage uses blob-name prefixes to represent folders. A folder such as `/root/Outputs` therefore corresponds to the `Outputs/` prefix in the container.
