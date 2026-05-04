# JSON overview

Flow includes built-in support for working with JSON — reading JSON content into a flow, flattening it into a tabular format for insertion into a database, and producing JSON files for upload or further processing. The most common use case is consuming JSON responses from REST APIs and writing the data into a relational database, but the actions also support file-based JSON workflows.

The actions in this category don't use a connection — they operate on in-memory JSON data passed in as a string, byte array, or stream.

<br/>

## Explore

#### Reading JSON
[Read JSON file as string](./read-json.md) takes a byte array or stream containing JSON and returns it as a string — useful when downstream actions need the raw text, or when you want to parse it manually using a [Function](../built-in/function.md) and the .NET JSON APIs.

<br/>

#### Converting JSON to tabular data
Two actions flatten a JSON document into rows and columns, ready to be inserted into a database or exported to other tabular formats like Excel or Parquet. [Convert JSON to DataTable](./get-json-datatable.md) loads everything into memory at once. [Get JSON DataReader](./get-json-datareader.md) returns a forward-only stream of rows. Both accept a string or byte array, support an optional **Schema mapping** for explicitly defining JSON properties, target column names, and data types, and support a **Root path** for cases where the array of records is nested inside the JSON rather than at the root level. The typical pattern is calling a REST API, then flattening the JSON response and inserting the result directly into a SQL Server table.

<br/>

#### Creating JSON files
[Create JSON file as stream](./create-json-file-as-stream.md) and [Create JSON file as byte array](./create-json-file-as-byte-array.md) take a JSON-formatted string and wrap it as a stream or byte array — typically as preparation for uploading to blob storage, sending over HTTP, or attaching to an email. Note that these actions take an already-formatted JSON string as input; they don't serialize .NET objects to JSON. To produce the JSON string itself, use a [Function](../built-in/function.md) with the .NET JSON APIs.
