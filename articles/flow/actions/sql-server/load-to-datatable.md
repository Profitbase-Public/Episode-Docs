# Load to DataTable

Loads data from a SQL Server database into memory as a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) object.

![Load to DataTable](https://profitbasedocs.blob.core.windows.net/flowimages/load-to-datatable.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow loads transactions to a DataTable. The DataTable is then used to create a [Parquet file](../parquet/create-parquet-file-as-stream.md) that can be saved as a file in, for example, [Azure file storage](../azure-files/upload-file.md).

<br/>

## Properties

| Name         | Required            | Description                                       |
|--------------|-----------------|---------------------------------------------------|
| **Title**    | No | A descriptive title for the action.               |
| **Connection** | Yes | The [SQL Server Connection](./connection.md) to the target database.         |
| **Dynamic connection** | No | When enabled, uses a connection created at runtime by [Create SQL Server Connection](./create-connection.md). Use this when the target database varies between runs. |
| **SQL expression and parameters** | Yes | The SQL query to execute, with optional parameters. Supports parameterized queries and Flow variable interpolation. See [Execute Command — How to build expressions and using parameters](./execute-command.md#how-to-build-expressions-and-using-parameters) for syntax. |
| **Table variable name** | Yes | The name of the variable returning the DataTable  |
| **Command timeout** | No | The time limit for command execution before it times out. Default is 120 seconds.|
| **Description** | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

[DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable).

<br/>

>[!NOTE]
> - The **Object Explorer** can only be used with a valid **Connection** and not **Dynamic connection**.
> - The **Object Explorer** cannot be used with a Hypergene Flow system database.

<br/>

## See also

- [Get DataReader](./get-datareader.md) — executes a query and returns the full result set as a DataReader for batch processing.
- [For Each Row from Query](./for-each-row-from-query.md) — iterates over rows one at a time with per-row logic.
- [Get Single Value](./get-single-value.md) — returns a single scalar value from a query.
- [Execute Command](./execute-command.md) — executes a SQL command without returning results.
- [Connection](./connection.md) — how to set up a SQL Server connection.


<br/>

[!INCLUDE [](__videos.md)]