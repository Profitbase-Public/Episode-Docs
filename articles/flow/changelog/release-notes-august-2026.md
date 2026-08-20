# Flow August 2026 update

The August 2026 release focuses on connector improvements, concurrency control, developer experience, and AI agent capabilities. On the connector side, we've added support for **Oracle NetSuite**, **Finago** adds paged REST API support, **Dynamics 365 Business Central** gets a general-purpose REST API Request action, and a new **AI/ML Anomaly Detection** node uses Isolation Forest with SHAP explanations to flag unusual rows in tabular data.  

Other highlights include the new **Harness AI agent** for long-running, multi-step agentic tasks, **AI agent skills** for extending agents with domain-specific knowledge and executable workflows, and **AI agent file access and file memory** nodes that give agents a SQL Server-backed virtual file system for storing outputs, plans, and working state. 
**Distributed semaphores** enable orchestration of parallel Flow executions, animated execution progress aids debugging in the Designer, and **packages can now be installed and upgraded directly from the Hypergene Store**. Access control gains a read-only permission option, and **MCP server configuration is now available at the Workspace level** for finer-grained scoping.

<br/>

## Oracle NetSuite connector
We've added support for building data integrations to Oracle NetSuite using the new [paged](../actions/netsuite/paged-rest-api-request.md) and [non-paged](../actions/netsuite/rest-api-request.md) actions for the NetSuite REST API.  
The addition of these two new actions makes enables building data integrations for both fetching large amounts or data (paged), as well as modifying (adding, updating and deleting) data in NetSuite.

![img](/images/flow/netsuite-toolbox-actions.png)

<br/>

## Finago - Support for paged REST API request
We've added support to the Finago connector for [Paged REST API request](../actions/finago-office/paged-rest-api-request.md) to handle large data sets such as the `Transaction lines` API.

![img](/images/flow/finagooffice-rest-api-paging.png)

<br/>

## Dynamics 365 - Support for non-paged API requests (create / update / delete / read entities)

We've added the [REST API Request](../actions/dynamics365/business-central/rest-api-request.md) action for the Dynamics 365 Business Central connector. Use it to call the Business Central v2 API with `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` to create, update, delete, or read records. The action supports dynamic connections, returns an [`HttpResponse<T>`](../../api-reference/built-in-types/http-response.md) with status code and error details, and automatically retries on `429 Too Many Requests`. For large data sets that require paging, use the [REST API Request with paging](../actions/dynamics365/business-central/rest-api-request-with-paging.md) action instead.

![img](/images/flow/dynamics365-bc-api-request.png)

<br/>

## Install and upgrade Packages from Hypergene Store
Flow packages are redistributable collections of pre-made Flows that can be installed in Workspaces. A typical example of a Flow package is an ERP data integration like the Visma Business NXT or Dynamics 365 ERP Package. 
In addition to files, you can now install and upgrade Flow packages from the Hypergene Store. This makes distribution of official packages much easier, as they can now be installed from a central repository instead of passed around as files.  

![Packages Install](/images/flow/packages-install.png)  

<br/>

## AI / ML - Anomaly detection
The new [Anomaly Detection](../actions/machine-learning/anomaly-detection.md) node identifies unusual rows in tabular numeric datasets using the Isolation Forest algorithm. Pass in a CSV or Parquet file, specify which numeric columns to analyse, and the node returns only the anomalous rows — each accompanied by a SHAP-based explanation that describes which features contributed most to the classification. Typical use cases include flagging fraudulent transactions, detecting sensor readings outside expected operating ranges, and surfacing data quality issues in large datasets.

<br/>

## Hypergene Portfolios API OAuth2 authentication support
Flow now supports the new [OAuth2 authentication option](../actions/hypergene-portfolios/connection.md#oauth2-client-credentials-authentication) for the Portfolios API.  

![img](/images/flow/portfolios-connection-oauth2.png)

<br/>

## Access control - option for read-only permissions
You can now grant specific users read-execute-only permissions, meaning they can open and execute Flows, but not make any changes. 
You'd typically apply this permission to users with responsibilities limited to inspecting logs and re-running Flows manually.

![img](/images/flow/user-readonly.png)

<br/>

## MCP at Workspace level
You can now specify MCP server settings at the Workspace level (in addition to the current Tenant level). This enables separate authentication settings pr Workspace, meaning MCP servers can be scoped by Workspace and not just by Tenant. A typical use case is to create a Workspace pr customer or team and expose AI tools to only a specific group of consumers.  
If no MCP Server settings is defined for a Workspace, the MCP setting at the Tenant level applies. 

<br/>

## Distributed semaphores

Flow now supports distributed semaphores for throttling and synchronizing concurrent Flow executions across a Workspace. Use the [Acquire semaphore](../actions/built-in/acquire-semaphore.md) action to limit how many Flows can run concurrently within a given scope, and the [Await semaphore drained](../actions/built-in/await-semaphore-drained.md) action to wait for all holders to finish before continuing. This makes it straightforward to fan out work across parallel Flows and consolidate results once all tasks are complete, without race conditions.

![img](/images/flow/await-semaphore-drained.png)

<br/>

## Harness AI agent

The new [Harness AI agent](../actions/agents/harness-ai-agent.md) defines an AI agent capable of performing long-running, complex, multi-step tasks using tools, skills, and external services such as file systems and APIs. Unlike the Tools AI agent, it is designed for open-ended, interactive workflows where the agent reasons through a task, uses tools to act, and writes its output to a backing store such as a SQL Server file access node for retrieval by downstream nodes. It supports two types of memory — conversation history for ongoing sessions with a user, and internal working memory for storing plans, notes, and intermediate files — and can stream its progress back to the client as it works.

![img](/images/flow/harness-ai-agent.png)

<br/>

## AI agent skill

The new [AI agent skill](../actions/agents/ai-agent-skill.md) lets you teach AI agents specialized knowledge and workflows — domain-specific processes, rules, and reference material that the model would not otherwise know. A skill consists of a name and description (always visible to the agent as a routing signal), plus instructions, scripts, and resources that are loaded into context only when the agent decides the skill is relevant to the current task. This progressive, on-demand loading is a key mechanism for keeping token usage and cost low: skills that aren't needed in a given run consume no tokens. Scripts are executable Flow functions the agent can invoke mid-task, while resources are named reference content (lookup tables, schemas, style guides) the agent can pull in on demand.

![img](/images/flow/ai-agent-skill.png)

<br/>

## AI agent file memory for SQL Server (virtual file system)

[AI agent file memory](../actions/sql-server/agent-file-memory.md) for SQL Server gives the [Harness AI agent](../actions/agents/harness-ai-agent.md) a SQL Server-backed store for working memory — notes, plans, intermediate state, and temporary files. This is separate from conversation history: it captures what the agent is *doing*, not what it has *said*. A practical benefit is that the agent can pause mid-task to ask the user for clarification or confirmation, then pick up exactly where it left off once the user responds. The backing table is created automatically on first use; no upfront schema setup is required.

![img](/images/flow/sql-server-agent-file-memory.png)

<br/>

## AI agent file access for SQL Server (virtual file system)

Use the [AI agent file access for SQL Server](../actions/sql-server/agent-file-access.md) node to expose a SQL Server table as a virtual file system where AI agents can create, read, write, search, and delete files and folders. Attach it to the `Context providers` port of a [Harness AI agent](../actions/agents/harness-ai-agent.md) to let the agent write its output to files — such as JSON or CSV — for retrieval by downstream nodes like [Load to DataTable](../actions/sql-server/load-to-datatable.md). You can also pre-populate the table with input files before running the agent, giving it structured context through a familiar file system API. As with the file memory node, the backing table is created automatically if it does not already exist.

![img](/images/flow/sql-server-agent-file-access.png)

<br/>

## AI agent file access for Azure Blob (virtual file system)
Use the [AI agent file access for Azure Blob](../actions/azure-blob-storage/agent-file-access.md) node to use an Azure Blob container as a virtual file system where AI agents can create, read, write, search, and delete files and folders. Attach it to the `Context providers` port of a [Harness AI agent](../actions/agents/harness-ai-agent.md) to let the agent write its output to files — such as JSON or CSV — for retrieval by downstream nodes like [Read blob as byte array](../actions/azure-blob-storage/read-blob-as-byte-array.md). You can also pre-populate the blob container with input files before running the agent, giving it structured context through a familiar file system API. As with the file memory node, the backing table is created automatically if it does not already exist.

![AI agent file access connected to a Harness AI agent](/images/flow/azure-blob-agent-file-access.png)

<br/>

## Animated Flow execution progress
To improve the developer experience in the Designer, nodes are now adorned by an animation while executing, and flagged with an `execution completed` icon when they have executed at least once (nodes in loop bodies are executed multiple times). This makes it easy to track the execution progress of Flows during development and testing.

![img](/images/flow/execute-flow-animate-progress.png)

<br/>


## Misc bug fixes and enhancements
- [Feature] CSV nodes: Create column mappings from clipboard
- [Feature] [Run Flow](../actions/built-in/run-flow.md) / [Start Flow](../actions/built-in/start-flow.md) nodes: Open the selected Flow in a new browser tab. Improves developer experience by easily allowing the developer to inspect the Flow that will be executed.
- [Bug fix] Fixed an issue where you could not select a Workspace in the Portal unless there were at least two Workspaces in the tenant
- [Bug fix] [Load Delta Table](../actions/sql-server/load-deltatable.md) handles square brackest in the db schema properly. It also contains other improvements such as handling joins (compare) on NULL columns and fixes a potential issue with hash signatures.
- [Bug fix] Default heights for some nodes were incorrect (too short)