# Convert Excel file to DataTable

Loads the contents of an Excel sheet into memory as a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable).

> [!NOTE]
> This action was previously named **Read Excel file as DataTable**.

![Read Excel File As Datatable](https://profitbasedocs.blob.core.windows.net/flowimages/read-excel-file-as-datatable.png)

<br/>

## Properties

| Name                | Required |Description     |
| ------------------- | -------- | --------------------------------------------- |
| Title                | No |   The title of the action.                    |
| File contents | Yes |  The contents of the file, provided as either a stream or a byte array. |
| Sheet name          | No | The name of the sheet(s) from which to read data. If you don't specify anything, data is read from the first sheet in the file. When specifying multiple sheets, use a semicolon to separate sheet names. In the rare case that a sheet name contains a semicolon, use square brackets around the sheet name.       |
| Data start row      | No | The row number of the first row to read. If the worksheet has a header row, `Data start row` is typically `2`.   |
| Column mapping      | Yes | Defines the mapping between the `column letters` (A,B,C, etc) in a worksheet, and the `column names` (CustomerID, Amount, etc) in the target DataTable. |
| Table variable name | No | The name of the DataTable variable returned from the action. |
| Disabled | No | When checked, the action is skipped during Flow execution. |
| Description | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

[DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable) containing data from all rows and columns as specified by the `Data start row` and `Column mapping` properties.

<br/>

### Automatic column mapping

Columns can be mapped automatically by using the "Map from sample file" option.

![Get Data Reader Ex2](https://profitbasedocs.blob.core.windows.net/flowimages/getDataReaderEx2.png)

<br/>

[!INCLUDE [](./__videos.md)]