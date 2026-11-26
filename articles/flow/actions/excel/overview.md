# Excel overview

Flow makes it easy to create data integrations with Microsoft Excel, enabling reading and writing Excel files from various data sources. The actions in this category cover both directions — reading rows from a worksheet into a database or a flow's logic, and generating Excel output from tabular data, either as a single sheet or as a workbook with multiple sheets — plus a Markdown converter useful for feeding spreadsheet content into AI workflows.

A typical pattern is exporting query results to an Excel file and uploading it to a file share or blob container as a scheduled report, or reading an Excel file received from an external system and importing its rows into a database.

<br/>

## Explore

#### Reading Excel files
Three actions read Excel input, and the right one depends on how much data you have and what you want to do with it. [Convert Excel file to DataReader](./open-excel-file-as-datareader.md) provides a forward-only stream of rows — typically used as input to actions like [SQL Server Insert Data](../sql-server/insert-data.md) for large imports. [Convert Excel file to DataTable](./read-excel-file-as-datatable.md) loads everything into memory at once. [For each row in Excel file](./for-each-row.md) iterates row by row when you want to apply per-row logic in the flow itself. All three accept multiple sheets at once (separated by `;`) and let you skip header rows using **Data start row**, with column mapping defined by Excel column letters (A, B, C, …).

<br/>

#### Creating Excel files
Generate an Excel file with a single sheet from a DataReader or DataTable. [Convert DataTable or DataReader to Excel file stream](./create-excel-file-as-stream.md) returns a stream — typical when uploading directly to blob/object storage or attaching to an email. [Convert DataTable or DataReader to Excel file bytes](./create-excel-file-as-byte-array.md) returns a byte array — useful when the same content needs to be reused. Both actions support specifying a sheet name, column mapping, and detailed worksheet formatting options.

<br/>

#### Creating workbooks with multiple sheets
When a file needs several sheets — each with its own data source, column mapping, and formatting — or when you want to add sheets to an existing Excel file, use the workbook actions. [Create or Open Excel workbook](./open-workbook.md) creates an empty workbook, or opens an existing `.xlsx` or `.xlsm` file, and exposes it in its **Use workbook** branch. Inside that branch, [Add sheet to Excel workbook](./add-sheet.md) appends a sheet from a DataTable or DataReader, and can be used several times or inside a loop. When all sheets are added, [Convert Excel workbook to file bytes](./convert-workbook-to-file-bytes.md) or [Convert Excel workbook to file stream](./convert-workbook-to-file-stream.md) produces the final file.

<br/>

#### Converting Excel to Markdown
[Convert Excel file to Markdown](./convert-to-markdown.md) takes an Excel file and produces a Markdown representation of its contents. The typical use case is preparing spreadsheet data for AI workflows — for example, splitting the Markdown into chunks, generating embeddings, and storing them for vector search alongside other documents.

<br/>

[!INCLUDE [](./__videos.md)]
