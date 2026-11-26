# Convert DataTable or DataReader to Excel file stream

Creates an Excel file from a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) or [DataReader](https://learn.microsoft.com/en-us/dotnet/api/system.data.idatareader) and returns the result as a [Stream](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream).

> [!TIP]
> This action creates a file with a single sheet. To create a file with multiple sheets, or to add sheets to an existing Excel file, use [Create or Open Excel workbook](./open-workbook.md) together with [Add sheet to Excel workbook](./add-sheet.md).


![Get Data Reader Ex1](../../../../images/flow/getDataReaderEx1.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow [exports sales data](../sql-server/get-entities.md) for September 2024 and uploads it as an Excel file. It begins by retrieving the sales records using a database reader, then passes the resulting data into the Convert DataTable or DataReader to Excel file stream action, which generates an Excel file as a stream. The stream is then sent to the [Upload Sales](../azure-blob-storage/upload-blob.md) Sept’24 action, where it is saved to storage under the specified blob name. This pattern is useful for automating scheduled reports, monthly data exports, and delivering Excel files to downstream systems or users.

<br/>

## Properties

| Name                 | Required | Description                                                                                                   |
| -------------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| Title                | No |   The title of the action.                    |
| Source               | Yes | Specifies the source of the data, which can either be a DataTable or DataReader.                              |
| Sheet name           | No | Specifies a worksheet name. |
| Column mapping       | No | Defines the mapping between Excel column letters and business object property names.                          |
| Worksheet formatting options | No | Specifies detailed formatting options.                                                                                                      |
| Stream variable name | No | The name of the variable you can use to reference the Stream in other actions.                |
| Disabled | No | When checked, the action is skipped during Flow execution. |
| Description | No |  Additional notes or comments about the action or configuration. |

<br/>

## Returns

Returns the Excel file as a [Stream](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream)

<br/>

## Output ports

| Port           | Description |
| -------------- | ----------- |
| **Use stream** | Runs while the stream is available. Connect uploads and other actions that read the stream here. |
| **Continue**   | Runs after the **Use stream** branch has completed and the stream has been closed. |

<br/>

### Automatic column mapping

Columns can be mapped automatically by using the "Map from sample file" option.

![Get Data Reader Ex2](https://profitbasedocs.blob.core.windows.net/flowimages/getDataReaderEx2.png)

<br/>

[!INCLUDE [](./__videos.md)]
