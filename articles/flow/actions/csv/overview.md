# CSV overview

Flow makes it easy to use CSV files to import and export data between databases, services, and cloud storage solutions such as Amazon S3 and Azure Blob containers. The actions in this category cover both directions — reading rows from a CSV into a database or a flow's logic, and generating CSV output from tabular data.

A typical pattern is reading a CSV file produced by an external system (or downloaded from blob storage) and inserting its rows into a SQL Server table, or the reverse — querying a database, converting the result to CSV, and uploading it to a file share or blob container.

<br/>

## Explore

#### Reading CSV files
Three actions read CSV input, and the right one depends on how much data you have and what you want to do with it. [Open CSV file as DataReader](./open-csv-file-as-datareader.md) provides a forward-only stream of rows — preferred for large files, and typically used as input to actions like [SQL Server Insert Data](../sql-server/insert-data.md). [Read CSV file as DataTable](./read-csv-file-as-datatable.md) loads everything into memory at once and is suitable for small to mid-sized files (under ~250 000 rows). [For each row in CSV file](./for-each-row.md) iterates row by row when you want to apply per-row logic in the flow itself.

<br/>

#### Creating CSV files
Generate a CSV file from a DataReader or DataTable. [Create CSV file as stream](./create-csv-file-as-stream.md) returns a stream — preferred for performance and memory use, and typical when uploading directly to blob/object storage. [Create CSV file as byte array](./create-csv-file-as-byte-array.md) returns a byte array — useful when the same content needs to be reused, for example chunking a large dataset and appending each chunk to a blob. Both actions support configuring header row, delimiters, quote character, date format, and decimal separator.

<br/>

#### Configuring how CSV is parsed
The reading actions share a set of [configuration properties](./configuration-properties/column-mapping.md) that control how the CSV is interpreted: [Column mapping](./configuration-properties/column-mapping.md) defines the relationship between fields in the file and columns in the resulting data set (including data types), [Data import options](./configuration-properties/data-import-options.md) control general parsing, date/number formatting, and error handling, and [Field parser](./configuration-properties/field-parser.md) lets you transform individual field values during import using a small C# function. The same options can also be supplied dynamically [as JSON](./json.md) at runtime.

<br/>

#### Handling bad data
Real-world CSV files often contain malformed values, missing fields, or unexpected formats. Enabling error handling in [Data import options](./configuration-properties/data-import-options.md) lets the action collect failed rows in a [BadData](./bad-data.md) property — readable as an enumerable or a DataReader — so you can dump the bad rows to a database or file and investigate without halting the import.

<br/>

[!INCLUDE [](__videos.md)]
