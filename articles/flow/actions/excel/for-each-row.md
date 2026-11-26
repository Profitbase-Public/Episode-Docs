# For each row in Excel file

Iterates over rows in an Excel sheet. Each row is represented by a business object as defined by the `Column mapping` property.

![Flow that iterates over each order row in an Excel file, creates an order in a Function and writes it to an Excel file](https://profitbasedocs.blob.core.windows.net/flowimages/redrum.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow processes an Excel file of orders one row at a time. **Read order** takes the Excel file as `fileContents` and iterates over the rows in the sheet given in `sheetName`. For each row, the **For each Order** branch runs with the current row as the `Order` business object, with properties defined by the `Column mapping`. Inside the branch, **Create order** ([Function](../built-in/function.md)) receives `Order` and builds `orderUpForInsert`, which **Insert order** ([Convert DataTable or DataReader to Excel file bytes](./create-excel-file-as-byte-array.md)) uses as `source` to create an Excel file as `byteArray`. When all rows have been processed, the Flow continues from **Continue**. Use this action when each row needs its own logic, such as validation, lookups, or one API call per row.

<br/>

## Properties

<!--prettier-ignore-->
| Name              | Required | Description                                                                                    |
| ----------------- | --------- | ---------------------------------------------------------------------------------------------- |
| Title                | No |   The title of the action.                    |
| File contents | Yes |  The contents of the file, provided as either a stream or a byte array. |
| Sheet name          | No | The name of the sheet(s) from which to read data. If you don't specify anything, data is read from the first sheet in the file. When specifying multiple sheets, use a semicolon to separate sheet names. In the rare case that a sheet name contains a semicolon, use square brackets around the sheet name.       |
| Data start row    | No | The row number of the first row to read. If the worksheet has a header row, `Data start row` is typically `2`.  |
| Column mapping    | Yes | Defines the mapping between Excel column letters and business object property names.           |
| Row variable name | No | The name of the Row (business object) variable.                                                |
| Row data type     | Read-only | Displays the name of the data type (business object) that contains the data of the row.        |
| Description       | No |  Additional notes or comments about the action or configuration.  |

<br/>

### Automatic column mapping

Columns can be mapped automatically by using the "Map from sample file" option.

![Get Data Reader Ex2](https://profitbasedocs.blob.core.windows.net/flowimages/getDataReaderEx2.png)


<br/>

[!INCLUDE [](./__videos.md)]