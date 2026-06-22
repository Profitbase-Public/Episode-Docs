# Insert rows

Inserts rows of data into a SQL Server table using a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) or a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) as the source. Uses SQL Server bulk copy for high-throughput loading.

Use this action when you need to load a large number of rows into a table from an upstream data source — a JSON file, a CSV import, a query result from another database, or any action that produces a DataReader or DataTable.

## When to use this

- To bulk-load rows from a file (JSON, CSV) into a SQL Server table after reading the file with a DataReader action.
- To insert query results from one database into a table in another database.
- To load data produced by upstream transformation actions into a staging or target table.
- When performance matters — this action uses bulk copy, which is faster than row-by-row inserts via [Execute Command](./execute-command.md).

## How it works

- **Input**: A DataReader or DataTable from an upstream action, and the name of the destination table.
- **Processing**: Reads rows from the source and bulk-inserts them into the destination table in batches. The batch size controls how many rows are sent per round-trip. The target table must already exist and its schema must match the source (see [Target table schema](#target-table-schema) below).
- **Output**: Optionally stores the number of inserted rows in a Flow variable specified by **Result variable name**.

> [!IMPORTANT]
> The target table must exist before this action runs. Use [Create Table](./create-table.md) or [Create Table from Source](./create-table-from-source.md) to set it up if needed.


<br/>

![Flow that reads a JSON file from storage, parses it into a DataReader, inserts the data into a SQL Server table, and deletes the source directory](../../../../images/flow/insert-rows.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow imports JSON data from file storage into a SQL Server table and cleans up afterward. [Read File from Share as a Stream](../azure-files/read-file-as-stream.md) reads the file from Azure Files. [Get JSON DataReader](../json/get-json-datareader.md) parses the JSON content into a DataReader. **Insert Data into Table** bulk-loads the rows into the target table. [Delete Directory](../azure-files/delete-directory.md) removes the source folder to keep storage tidy. Use this pattern for file-based data imports where the source format is parsed into a DataReader before loading.

<br/>

## Properties

| Name                  | Required | Description                                                                 |
|-----------------------|----------|-----------------------------------------------------------------------------|
| **Title**                 | No | A descriptive title for the action.                                         |
| **Connection**            | Yes | The [SQL Server Connection](./connection.md) to the target database.                               |
| **Enable dynamic connection**    | No | When enabled, uses a connection created at runtime by [Create Connection](./create-connection.md). Use this when the target database varies between runs. |
| **Source**                | Yes | The data to insert. Accepts a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) or a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) from an upstream action.                |
| **Destination table**     | Yes | The name of the table to insert data into.                  |
| **Batch size**            | No | The number of rows inserted per batch. Default is 5000. Larger batches reduce round-trips but use more memory. The optimal value depends on row size and network latency. |
| **Result variable name**  | No | The name of a Flow variable that receives the number of rows inserted. Use this to pass the count to downstream actions or conditions.     |
| **Command timeout (seconds)** | No | Maximum execution time in seconds. The action fails with a timeout error if exceeded. Default is 120 seconds. |
| **Disabled** | No | When checked, the action is skipped during Flow execution. |
| **Description**           | No | Additional notes or comments about the action or configuration.              |

<br/>

## Target table schema

The target table must have a schema (columns and data types) that matches the schema of the **Source** DataReader or DataTable. The columns do not need to be in the same order, but they must match by name and data type.

> [!NOTE]
> If you get the error **"The locale id '...' of the source column '...' and the locale id '...' of the destination column '...' do not match"**, the collation differs between source and target columns. Two options:
>
> - Use a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) instead of a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) as the **Source**.
> - Convert the collation in the source query to match the target. For example: `SELECT CONVERT(varchar(100), [ColumnName] COLLATE target_collation_name) AS [ColumnName] FROM ...` in the [Get DataReader](./get-datareader.md) action that produces the source.

<br/>

## Returns

No direct return value. If **Result variable name** is set, the specified Flow variable receives the total number of rows inserted.

## See also

- [Insert or Update Row](./insert-or-update-row.md) — upserts a single row based on key match.
- [Merge Tables](./merge-tables.md) — merges source rows into a target table with insert/update/delete logic.
- [Load to DataTable](./load-to-datatable.md) — loads data into a DataTable for use as a source.
- [Get DataReader](./get-datareader.md) — executes a query and returns a DataReader for use as a source.
- [Create Table](./create-table.md) — creates the target table before inserting.
- [Connection](./connection.md) — how to set up a SQL Server connection.

<br/>

[!INCLUDE[](__videos.md)]