# Convert Excel file to DataReader

Provides a [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) for reading a forward-only stream of rows from an Excel sheet.

![Flow where customer data is read from an Excel file with a DataReader, exported as a Parquet file and uploaded to Azure Blob Storage](https://profitbasedocs.blob.core.windows.net/flowimages/getDataReaderEx.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow converts customer data from an Excel file to a Parquet file. **Read Customer data** takes the Excel file as `fileContents`, reads rows from the sheet given in `sheetName`, and provides them as a `reader`. The reader is used as `source` by **Export Customer data as Parquet** ([Create Parquet file as stream](../parquet/create-parquet-file-as-stream.md)), which writes the rows to the Parquet stream `parquetData`. In its **Use stream** branch, **Upload Customer Parquet File** ([Upload Blob](../azure-blob-storage/upload-blob.md)) uploads the stream to Azure Blob Storage. Because the DataReader reads one row at a time, this pattern also works for large Excel files.

<br/>

## Properties

| Name                 | Required | Description                                                                                                   |
| -------------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| Title                | No |   The title of the action.                    |
| File contents | Yes |  The contents of the file, provided as either a stream or a byte array. |
| Sheet name           | No | The name of the sheet(s) from which to read data. If you don't specify anything, data is read from the first sheet in the file. When specifying multiple sheets, use a semicolon to separate sheet names. In the rare case that a sheet name contains a semicolon, use square brackets around the sheet name.       |
| Data start row       | No | The row number of the first row to read. If the worksheet has a header row, `Data start row` is typically `2` |
| Column mapping       | Yes | Defines the mapping between Excel column letters and business object property names.                          |
| Reader variable name | No | The name of the variable you can use to reference the DataReader in other actions.                |
| Disabled | No | When checked, the action is skipped during Flow execution. |
| Description | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

A [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) for reading the rows and columns specified by the `Data start row` and `Column mapping` properties.

<br/>

### Automatic column mapping

Columns can be mapped automatically by using the "Map from sample file" option.

![Get Data Reader Ex2](https://profitbasedocs.blob.core.windows.net/flowimages/getDataReaderEx2.png)


<br/>

[!INCLUDE [](./__videos.md)]