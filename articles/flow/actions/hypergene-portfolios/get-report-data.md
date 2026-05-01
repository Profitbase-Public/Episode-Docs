# Get report data

Gets the raw data for a Hypergene Porfolios report in the specified supported format (JSON, CSV, XML or HTML).

> [!NOTE]
> If you request a format not supported by the report, the data will be returned in the report’s default format. For example, if you request CSV but the report only supports JSON, the data will be returned as JSON.

![Portfolio Get Report Data](/images/flow/portfolio-get-report-data.png)  

**Example** ![Example](../../../../images/strz.jpg)  
The example above illustrates how to import data from Hypergene Portfolios into your own Azure SQL database. It first fetches data in **CSV format** using the `Get report data` action, then converts it to a DataTable using the [Read CSV file as DataTable](../csv/read-csv-file-as-datatable.md) action and finally stores the data to an Azure SQL Database table using the [Insert rows](../sql-server/insert-data.md) action. The Portfolio API returns data as a string, so we must do the DataTable conversion manually to get the data to a tabular format that SQL Server understands.

<br/>

## Properties
| Name              |  Required         | Description                      |
|-------------------|-------------------|----------------------------------|
| Connection        | Yes | The [connection](./connection.md) to the Hypergene Portfolios API. |
| Portfolio         | No | When specified, overrides the Portfolio ID specified in the Connection. This can be a hard coded string, a variable or the Porfolio selected from the Portfolio picker in the property editor.  |
| Report            | Yes | The id of the report to get data for. This can be a hard coded numeric value, a variable, or the report selected from the Report picker in the property editor. |
| Format            | No | The preferred format for the returned data. Options include `JSON`, `CSV`, `HTML`, and `XML`, though not all reports support every format. You can select from the list of available formats when configuring this property. If you specify a format that the report doesn’t support (for example, when using a variable), the data will be returned in the report’s default format. |

<br/>

## Returns
Returns data as a [string](https://learn.microsoft.com/en-us/dotnet/api/system.string) in the specified format (JSON, CSV, XML, or HTML). If the specified format is not supported by the report, data will be returned in the report’s default format.  

<br/>

[!INCLUDE [](./__videos.md)]