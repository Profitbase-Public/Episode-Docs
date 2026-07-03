# Flow August 2026 update

The August 2026 release focuses on concurrency control, developer experience, and connector improvements. Highlights include distributed semaphores for orchestrating parallel Flow executions, animated execution progress in the Designer, and the ability to install and upgrade packages directly from the Hypergene Store. Access control gains a read-only permission option, and MCP server configuration is now available at the Workspace level for finer-grained scoping. On the connector side, Finago adds paged REST API support, Dynamics 365 Business Central gets a general-purpose REST API Request action, and a new AI/ML Anomaly Detection node uses Isolation Forest with SHAP explanations to flag unusual rows in tabular data.

## Hypergene Portfolios API OAuth2 authentication support
Flow now supports the new [OAuth2 authentication option](../actions/hypergene-portfolios/connection.md#oauth2-client-credentials-authentication) for the Portfolios API.  

![img](/images/flow/portfolios-connection-oauth2.png)

<br/>

## Distributed semaphores

Flow now supports distributed semaphores for throttling and synchronizing concurrent Flow executions across a Workspace. Use the [Acquire semaphore](../actions/built-in/acquire-semaphore.md) action to limit how many Flows can run concurrently within a given scope, and the [Await semaphore drained](../actions/built-in/await-semaphore-drained.md) action to wait for all holders to finish before continuing. This makes it straightforward to fan out work across parallel Flows and consolidate results once all tasks are complete, without race conditions.

![img](/images/flow/await-semaphore-drained.png)

<br/>

## Animated Flow execution progress
To improve the developer experience in the Designer, nodes are now adorned by an animation while executing, and flagged with an `execution completed` icon when they have executed at least once (nodes in loop bodies are executed multiple times). This makes it easy to track the execution progress of Flows during development and testing.

![img](/images/flow/execute-flow-animate-progress.png)

<br/>

## Install and upgrade Packages from Hypergene Store
Flow packages are redistributable collections of pre-made Flows that can be installed in Workspaces. A typical example of a Flow package is an ERP data integration like the Visma Business NXT or Dynamics 365 ERP Package. 
In addition to files, you can now install and upgrade Flow packages from the Hypergene Store. This makes distribution of official packages much easier, as they can now be installed from a central repository instead of passed around as files.  

![Packages Install](/images/flow/packages-install.png)  

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

## Finago - Support for paged REST API request
We've added support to the Finago connector for [Paged REST API request](../actions/finago-office/paged-rest-api-request.md) to handle large data sets such as the `Transaction lines` API.

![img](/images/flow/finagooffice-rest-api-paging.png)

<br/>

## Dynamics 365 - Support for non-paged API requests (create / update / delete / read entities)

We've added the [REST API Request](../actions/dynamics365/business-central/rest-api-request.md) action for the Dynamics 365 Business Central connector. Use it to call the Business Central v2 API with `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` to create, update, delete, or read records. The action supports dynamic connections, returns an [`HttpResponse<T>`](../../api-reference/built-in-types/http-response.md) with status code and error details, and automatically retries on `429 Too Many Requests`. For large data sets that require paging, use the [REST API Request with paging](../actions/dynamics365/business-central/rest-api-request-with-paging.md) action instead.

![img](/images/flow/dynamics365-bc-api-request.png)

<br/>

## AI / ML - Anomaly detection
The new [Anomaly Detection](../actions/machine-learning/anomaly-detection.md) node identifies unusual rows in tabular numeric datasets using the Isolation Forest algorithm. Pass in a CSV or Parquet file, specify which numeric columns to analyse, and the node returns only the anomalous rows — each accompanied by a SHAP-based explanation that describes which features contributed most to the classification. Typical use cases include flagging fraudulent transactions, detecting sensor readings outside expected operating ranges, and surfacing data quality issues in large datasets.

<br/>

## Misc bug fixes and enhancements
- [Feature] CSV nodes: Create column mappings from clipboard
- [Feature] [Run Flow](../actions/built-in/run-flow.md) / [Start Flow](../actions/built-in/start-flow.md) nodes: Open the selected Flow in a new browser tab. Improves developer experience by easily allowing the developer to inspect the Flow that will be executed.
- [Bug fix] Fixed an issue where you could not select a Workspace in the Portal unless there were at least two Workspaces in the tenant
- [Bug fix] [Load Delta Table](../actions/sql-server/load-deltatable.md) handles square brackest in the db schema properly
- [Bug fix] Default heights for some nodes were incorrect (too short)