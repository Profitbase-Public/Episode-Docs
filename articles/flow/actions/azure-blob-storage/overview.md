# Azure Blob Storage overview

Flow has extensive support for [Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction), including listing, reading, writing, and deleting blobs, retrieving blob metadata, creating connections dynamically at runtime, and exposing Blob Storage to AI agents as a tool.

To use any Blob Storage action, you first need an [Azure Blob container connection](./azure-blob-container-connection.md). Flow supports two authentication types: a [SAS URI](./azure-blob-container-connection.md#sas-uri) for time-limited, scoped access, or a [Connection string with container name](./azure-blob-container-connection.md#connection-string-and-container-name) for broader or non-expiring access. The same connection is reused across all actions, or you can build one dynamically with [Create Azure Blob container connection](./create-azure-blob-container-connection.md) when credentials live outside Flow.

<br/>

## Explore

#### Connecting to Azure Blob Storage
Set up a static [connection](./azure-blob-container-connection.md) using SAS URI or connection string, or use [Create Azure Blob container connection](./create-azure-blob-container-connection.md) to build a connection at runtime — useful when credentials are stored in your own database and need to be selected based on flow parameters.

<br/>

#### Listing blobs
Get the contents of a container, with optional prefix filtering. Use [Get Blob names](./get-blob-names.md) for a list of names as a variable, or [For each Blob name](./foreach-blob-name.md) to iterate one at a time. When you also need metadata (size, last modified, content type, etc.), use [Get Blob info](./get-blob-info.md) for a single blob or [For each Blob info](./foreach-blob-info.md) to iterate metadata for many — for example, to process only blobs modified today.

<br/>

#### Reading blobs
Download a blob's contents into a flow. [Read Blob as stream](./read-blob-as-stream.md) is preferred for performance and lower memory use; [Read Blob as byte array](./read-blob-as-byte-array.md) is required when the same content needs to be read more than once. Once read, the contents must be loaded with a compatible action — such as those in [Excel](../excel/overview.md) or [CSV](../csv/overview.md) — before the data can be used.

<br/>

#### Writing and removing blobs
[Upload Blob](./upload-blob.md) creates a new blob from a byte array or stream. [Append to Blob](./append-to-blob.md) adds data to an existing blob, or creates it if it doesn't exist yet. [Delete Blob](./delete-blob.md) removes a blob from the container.

<br/>

#### Using Blob Storage from an AI agent
[Azure Blob Storage Agent Tool](./agent-tool.md) lets a [Tools AI Agent](../agents/tools-ai-agent.md) work with a container autonomously — listing, reading, writing, or deleting blobs based on the agent's reasoning. Capabilities are configurable, so you can restrict the agent to only the operations it actually needs.

<br/>

[!INCLUDE [](./__videos.md)]
