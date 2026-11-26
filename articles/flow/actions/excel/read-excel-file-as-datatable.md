# Convert Excel file to DataTable

Loads the contents of an Excel sheet into memory as a [DataTable](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable).

![Flow where order data is loaded from an Excel file into a DataTable, prepared in a Function and written to a new Excel file](https://profitbasedocs.blob.core.windows.net/flowimages/read-excel-file-as-datatable.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow reads order data from an Excel file, prepares it, and writes the result to a new Excel file. **Read order** takes the Excel file as `fileContents`, reads the sheet given in `sheetName`, and loads all rows into memory as the DataTable `TablesOrder`. **Prepare order** ([Function](../built-in/function.md)) receives `TablesOrder` and transforms the data into `orderUpForInsert`. **Insert order** ([Convert DataTable or DataReader to Excel file bytes](./create-excel-file-as-byte-array.md)) then uses the prepared data as `source` and creates a new Excel file as `byteArray`. Use a DataTable when you need all rows at once, for example to sort, filter, or look up values across rows before passing the data on.

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