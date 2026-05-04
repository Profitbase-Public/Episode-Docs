# Create Parquet file as byte array

Creates a Parquet file from a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) or [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) and returns the result as a byte array.

<br/>

**Example**

Return value used:

![Parquet As Byte Array1](https://profitbasedocs.blob.core.windows.net/flowimages/parquet-as-byte-array1.png)

<br/>

## Properties

| Name                 | Required | Description                                                                                                                                                                          |
| -------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Title    | No |      The title for the create action.               |
| Source               | Yes | Specifies the source of the data, which can either be a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) or [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable). |
| Result variable name | No | The name of the variable you can use to reference the byte array in other actions.  |
| Description          | No |  Additional notes or comments about the action or configuration. |

<br/>

## Returns

Returns the Parquet file serialized as a [byte](https://learn.microsoft.com/en-us/dotnet/api/system.byte) [array](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/arrays)

<br/>

![Parquet As Byte Array2](https://profitbasedocs.blob.core.windows.net/flowimages/parquet-as-byte-array2.png)
