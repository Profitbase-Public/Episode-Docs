# Excel overview

Flow makes it easy to create data integrations with Microsoft Excel, enabling reading and writing Excel files from various data sources. The actions in this category cover both directions — reading rows from a worksheet into a database or a flow's logic, and generating Excel output from tabular data — plus a Markdown converter useful for feeding spreadsheet content into AI workflows.

A typical pattern is exporting query results to an Excel file and uploading it to a file share or blob container as a scheduled report, or reading an Excel file received from an external system and importing its rows into a database.

<br/>

## Explore

#### Reading Excel files
Three actions read Excel input, and the right one depends on how much data you have and what you want to do with it. [Open Excel file as DataReader](./open-excel-file-as-datareader.md) provides a forward-only stream of rows — typically used as input to actions like [SQL Server Insert Data](../sql-server/insert-data.md) for large imports. [Read Excel file as DataTable](./read-excel-file-as-datatable.md) loads everything into memory at once. [For each row in Excel file](./for-each-row.md) iterates row by row when you want to apply per-row logic in the flow itself. All three accept multiple sheets at once (separated by `;`) and let you skip header rows using **Data start row**, with column mapping defined by Excel column letters (A, B, C, …).

<br/>

#### Creating Excel files
Generate an Excel file from a DataReader or DataTable. [Create Excel file as stream](./create-excel-file-as-stream.md) returns a stream — typical when uploading directly to blob/object storage or attaching to an email. [Create Excel file as byte array](./create-excel-file-as-byte-array.md) returns a byte array — useful when the same content needs to be reused. Both actions support specifying a sheet name, column mapping, and detailed worksheet formatting options.

<br/>

#### Converting Excel to Markdown
[Convert an Excel file to Markdown](./convert-to-markdown.md) takes an Excel file and produces a Markdown representation of its contents. The typical use case is preparing spreadsheet data for AI workflows — for example, splitting the Markdown into chunks, generating embeddings, and storing them for vector search alongside other documents.

<br/>

[!INCLUDE [](./__videos.md)]
