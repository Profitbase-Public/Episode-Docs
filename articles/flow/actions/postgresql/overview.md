# PostgreSQL overview

Flow includes built-in support for working with [PostgreSQL](https://www.postgresql.org/) databases — reading data in several shapes (DataReader, DataTable, typed entities, single values), writing rows individually or in bulk, executing arbitrary SQL commands, and storing/searching embedding vectors for RAG (Retrieval-Augmented Generation) workflows. Among the database categories in Flow, PostgreSQL is currently the most complete option for vector-based AI workflows, with dedicated actions covering the full save-and-search pipeline.

To use any PostgreSQL action, you first need a [PostgreSQL connection](./postgresql-connection.md). Flow supports two authentication options: standard **PostgreSQL Authentication** (server, database, username, password) or a **Custom Connection String** for cases where you need full control over the connection details.

<br/>

## Explore

#### Connection
Set up the connection used by every PostgreSQL action — pick between PostgreSQL Authentication or a custom connection string, and optionally configure a connection timeout.  
[Read more](./postgresql-connection.md)

<br/>

#### Reading data
Five actions execute a SQL query and return its result, each in a different shape — pick the one that fits how you want to consume the data downstream. [Get DataReader](./get-datareader.md) returns a forward-only stream of rows. [Load to DataTable](./load-to-datatable.md) loads everything into memory at once. [Get entity](./get-entity.md) returns a single entity mapped to a typed object, and [Get entities](./get-entities.md) returns a list of typed entities — both useful when you want to work with strongly-typed data in subsequent actions. [Get single value](./get-single-value.md) executes a query that returns one scalar value (a count, a max, a single field). All five accept a parameterized SQL expression and a configurable command timeout (default 120 seconds).

<br/>

#### Writing data
Three actions cover write operations. [Insert rows](./insert-data.md) inserts data from a DataReader or DataTable into a target table using bulk insert — used for high-volume loads such as importing a CSV or query result. [Insert or update row](./upsert-row.md) performs an upsert on a single row defined through column-value mappings, useful when you don't know in advance whether the row already exists. [Execute command](./execute-command.md) runs an arbitrary SQL command (UPDATE, DELETE, custom INSERT, etc.) and returns the number of rows affected.

<br/>

#### Working with vectors
Two actions support storing and querying embeddings, intended for building RAG systems against PostgreSQL. [Save vectors](./vector-save.md) takes a text input and runs the full save pipeline — split into chunks using a [Text splitter](../ai/text-splitter.md), generate embeddings using a [Text embedder](../azure-ai/text-embedder.md), and upsert the resulting records into a PostgreSQL collection. [Search vectors](./vector-search.md) performs a vector similarity search against a table for a given input text, with optional filter expression, top, and skip — and returns an `IVectorSearchResult` object that can be passed directly to a Chat completion action as context. Together these enable a complete RAG pipeline (read documents → save vectors → vector search → chat completion) without leaving the PostgreSQL category for the vector storage layer.
