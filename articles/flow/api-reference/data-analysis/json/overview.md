# JSON overview

The **JSON** helpers in Data Analysis make it easy to flatten and read JSON content from inside a [Function](./../../../actions/built-in/function.md) action — when the JSON-handling [actions](./../../../actions/json/overview.md) don't fit and you want to drive the parsing yourself in C#. The classes implement the standard .NET data-reading interfaces, so their output can be piped directly into any action that accepts a DataReader.

<br/>

## Explore

#### Reading JSON as rows
[JsonDataReader](./json-data-reader/json-data-reader.md) reads a JSON string as a forward-only sequence of rows, implementing `IDataReader` and `IDataRecord`. Use it when you want to flatten a JSON document into tabular form — for example to insert it into a database table or pipe it through another DataReader-accepting action.

<br/>

#### Defining the schema
[JsonDataReaderSchema](./json-data-reader/json-data-reader-schema.md) defines how JSON properties map to columns: the JSON property name to read, the column name in the output, and the target data type. Pass an instance of this to JsonDataReader to control exactly which fields are extracted and how they're typed.
