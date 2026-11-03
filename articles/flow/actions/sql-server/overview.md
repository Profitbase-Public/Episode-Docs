# SQL Server and Azure SQL overview

Flow includes built-in support for working with [SQL Server](https://learn.microsoft.com/en-us/sql/sql-server/) and [Azure SQL](https://learn.microsoft.com/en-us/azure/azure-sql/) — the most extensive database category in Flow. It covers the full lifecycle of working with a relational database: managing schema (creating, checking, dropping, and truncating tables), reading data in many shapes (DataReader, DataTable, typed entities, single values, row-by-row iteration), writing data (bulk insert, typed insert/update, upsert, arbitrary SQL commands), change-tracking (DeltaTable, MERGE, Power BI writeback), explicit transactions, vector storage and search for RAG, and AI agent memory.

To use any SQL Server action, you first need a [SQL Server connection](./connection.md). Flow supports four authentication types: **SQL Server Authentication** (username and password), **Microsoft Entra Password**, **Microsoft Entra Service Principal**, and a **User Connection String** for full control. When credentials need to be selected at runtime — for example, to target a different server based on Flow input — use [Create SQL Server Connection](./create-connection.md) to build a dynamic connection.

<br/>

## Explore

### Connection
Set up the connection used by every SQL Server action — pick the authentication type that fits the target environment, or use [Create SQL Server Connection](./create-connection.md) to build a connection at runtime when credentials live outside Flow.

<br/>

### Managing schema
Several actions let flows manage the schema of a database. [Check if table exists](./check-if-table-exists.md) returns a boolean for use in conditional logic. [Create table](./create-table.md) creates a table from a column definition (does nothing if the table already exists). [Create table from source](./create-table-from-source.md) creates a copy of an existing table or view by copying its schema (data is not copied). [Delete table](./delete-table.md) drops a table (does nothing if it doesn't exist), and [Truncate table](./truncate-table.md) empties an existing one without dropping it. These are useful for staging tables, ETL initialization steps, and ensuring downstream actions have the structure they expect.

<br/>

### Reading data
Six actions execute a query and return the result, each in a different shape. [Get DataReader](./get-datareader.md) returns a forward-only stream — the right choice for large result sets that shouldn't be loaded entirely into memory. [Load to DataTable](./load-to-datatable.md) loads everything into memory at once. [Get entity](./get-entity.md) returns a single typed entity, and [Get entities](./get-entities.md) returns a list — both useful when downstream actions expect strongly typed data. [Get single value](./get-single-value.md) returns one scalar value (count, status flag, specific field). [For each row from query](./for-each-row-from-query.md) iterates the result one row at a time when you want to apply per-row logic in the Flow itself.

<br/>

### Writing data
Five actions cover the write paths. [Insert rows](./insert-data.md) bulk-inserts data from a DataReader or DataTable — used for high-volume loads such as importing a CSV or query result. [Insert entity](./insert-entity.md) inserts a single row from a typed entity, mapping each property name to a column in the target table. [Update entity](./update-entity.md) updates a row using a typed entity and one or more **Update keys** that identify which row to modify. [Insert or Update row](./insert-or-update-row.md) performs an upsert based on column-value mappings, useful when you don't know in advance whether the row already exists. [Execute command](./execute-command.md) runs an arbitrary SQL command (UPDATE, DELETE, custom DDL, etc.) and returns the number of rows affected.

<br/>

### Change tracking and merging
Three actions support detecting and applying changes between data sets. [Load DeltaTable](./load-deltatable.md) compares a source table with a target table and produces a **DeltaTable** containing only the inserted, updated, and deleted rows (with a `__rowState` column indicating which is which) — optionally in **Incremental Historic mode** that groups successive change sets by batch number. The resulting DeltaTable can then be used to update the target. [Merge tables](./merge-tables.md) is a lightweight wrapper around the SQL Server `MERGE` statement for source-into-target merging — for complex MERGE logic, use Execute command with custom SQL instead. [Save DeltaSet](./save-deltaset.md) applies row-level changes (inserts, updates, deletes) from a [DeltaSet](../../api-reference/built-in-types/deltaset.md#deltaset) to a target table — typically used to persist user edits made through the Power BI [Writeback Table](../../../PowerBI/writeback-table/overview.md) or [Writeback Comments](../../../PowerBI/writeback-comments/overview.md) visuals.

<br/>

### Working with vectors
Three actions support storing and querying embeddings for RAG workflows on SQL Server or Azure SQL. [Save vectors](./vector-save.md) takes a text input and runs the full pipeline — split the text into chunks with a [Text splitter](../ai/text-splitter.md), generate embeddings with a [Text embedder](../azure-ai/text-embedder.md), and upsert the resulting records into a SQL Server vector collection. [Search vectors](./search-vectors.md) performs a vector similarity search directly in the Flow and returns an `IVectorSearchResult` for downstream actions. [Vector search tool](./vector-search-tool.md) exposes the search to a chat completion or AI agent, allowing the LLM to decide when and how to retrieve context at runtime. Together, these actions enable complete RAG pipelines on SQL Server, alongside the [PostgreSQL equivalent](../postgresql/overview.md).

<br/>

### AI agent memory and files
Three actions provide persistent storage for AI agents in SQL Server. [AI chat history memory](./agent-memory.md) stores and retrieves conversation history so an agent can maintain context across turns. [AI agent file memory](./agent-file-memory.md) lets an agent preserve working files between requests, while [AI agent file access](./agent-file-access.md) gives the agent file-system-style access to SQL Server-backed files. Use these actions when agents need to resume work, exchange files with other Flow actions, or retain context beyond a single request.

<br/>

### Transactions
[Transaction scope](./transaction-scope.md) wraps a sequence of SQL operations in a single transaction, so they all succeed or all fail. The action accepts a configurable timeout and isolation level (default `Serializable`) and is the right tool when multiple writes must be applied atomically.

<br/>

[!INCLUDE [](./__videos.md)]
