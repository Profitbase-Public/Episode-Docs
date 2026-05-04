# Snowflake overview

Flow includes built-in support for working with [Snowflake](https://docs.snowflake.com/) — reading query results in different shapes (DataReader, DataTable, single value), writing data either by streaming it from memory or by bulk-loading from a staged file, and persisting Power BI writeback changes through DeltaSets. Common use cases include moving data between Snowflake and other systems (such as SQL Server, Azure Blob Storage, or Amazon S3), running scheduled SQL operations, and saving user edits from Power BI directly into Snowflake tables.

To use any Snowflake action, you first need a [Snowflake connection](./connecting-to-snowflake.md). Flow supports four authentication types: **Username and Password** (Snowflake authentication, marked for deprecation — avoid for new integrations), **Programmatic Access Token**, **Key Pair**, and **Custom connection string**. The connection identifies the Snowflake account and host, and is reused across actions.

<br/>

## Explore

#### Connection
Set up the connection used by every Snowflake action — pick the authentication type that fits the target environment. Username and Password authentication is being deprecated, so prefer Programmatic Access Token, Key Pair, or a custom connection string for new connections.  
[Read more](./connecting-to-snowflake.md)

<br/>

#### Reading data
Three actions execute a SQL query and return the result, each in a different shape — pick the one that fits how you want to consume the data downstream. [Get DataReader](./get-datareader.md) returns a forward-only stream of rows, well suited for large result sets that shouldn't be loaded entirely into memory. [Load to DataTable](./load-datatable.md) loads the full result set into memory at once, useful when you need to work with the data as a whole. [Get single value](./get-single-value.md) executes a query that returns one scalar value (a count, status flag, or specific field) and casts it to the target .NET type. All three accept parameterized SQL and a configurable command timeout (default 120 seconds).

<br/>

#### Writing data
Three actions cover the write paths into Snowflake. [Insert rows](./insert-data.md) inserts data from a DataReader or DataTable into a target table — used when the data is already in memory or streaming from another source. [Copy into](./copy-into.md) loads data into a Snowflake table from a [stage](https://docs.snowflake.com/en/sql-reference/sql/create-stage) (a file location inside or outside Snowflake, such as an Azure Blob Storage or Amazon S3 stage), supporting CSV, JSON, and Parquet — used when the data is already staged as files. [Execute command](./execute-non-query-command.md) runs an arbitrary SQL command (UPDATE, DELETE, custom DDL, etc.) and returns the number of rows affected. The typical pattern for large external data: produce a CSV/Parquet file in a flow, upload it to a stage, and run Copy into.

<br/>

#### Power BI writeback
[Save DeltaSet](./save-deltaset.md) applies a [DeltaSet](../../api-reference/built-in-types/deltaset.md#deltaset) — a set of row-level inserts, updates, and deletes — to a Snowflake table. This is the standard way to persist user edits made through the Power BI [Writeback Table](../../../PowerBI/writeback-table/overview.md) visual back into Snowflake. The action is typically used together with the [Writeback Table trigger](../../triggers/power-bi/writeback-table-trigger.md), which delivers the DeltaSet to the flow whenever users save changes in Power BI.
