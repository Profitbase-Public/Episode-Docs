# Parquet overview

Flow includes built-in support for working with [Parquet](https://parquet.apache.org/) files — reading their contents into a flow as tabular data, and generating Parquet files from a DataReader or DataTable. A typical use case is exporting query results into a Parquet file for downstream analytics workloads (such as a data lake or Microsoft Fabric Lakehouse), or reading Parquet files produced by other systems and feeding the rows into a database or another format.

The actions in this category don't use a connection — they operate on Parquet content passed in as a stream or byte array, typically read from or written to a storage system like [Azure Blob Storage](../azure-blob-storage/overview.md) or [Amazon S3](../amazon-s3/overview.md).

<br/>

## Explore

#### Reading Parquet files
Three actions read Parquet input. [Open Parquet file as DataReader](./open-parquet-file-as-datareader.md) provides a forward-only stream of rows. [Read Parquet file as DataTable](./read-parquet-file-as-datatable.md) loads the contents into memory at once. [For each row in Parquet file](./for-each-row.md) iterates row by row when you want to apply per-row logic in the flow itself. All three accept the file as a stream or byte array, and all three support **Column mapping** — used to rename columns, set their data types, or exclude unwanted columns from the output. Mappings can be defined statically in the action's configuration, or generated dynamically by a [Function](../built-in/function.md) returning a list of `ParquetColumnMapping` objects (useful when the same flow needs to handle different schemas).

<br/>

#### Creating Parquet files
[Create Parquet file as stream](./create-parquet-file-as-stream.md) and [Create Parquet file as byte array](./create-parquet-file-as-byte-array.md) generate a Parquet file from a DataReader or DataTable. Both accept the same source types and differ only in the output shape — stream is typical when the file is being uploaded directly to storage, byte array when the result needs to be reused.
