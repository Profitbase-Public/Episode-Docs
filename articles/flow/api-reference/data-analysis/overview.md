# Data Analysis overview

The **Data Analysis** APIs provide helper classes for transforming and reading data inside a [Function](./../../actions/built-in/function.md) action — when the standard actions in a category don't cover what you need and you want to write the logic yourself in C#. The classes are designed to integrate naturally with .NET's `DataTable`, `IDataReader`, and similar built-in types.

<br/>

## Explore

#### Transforming DataTables
[DataTableTransformer](./datatable-transformer/datatable-transformer.md) applies a sequence of transformations to a `DataTable` — renaming columns, changing data types, replacing values, removing rows. Accessed by calling the `UseTransform()` extension method on any `DataTable` instance.

<br/>

#### Reading JSON
[JSON](./json/overview.md) provides helpers for flattening JSON content into rows and columns. [JsonDataReader](./json/json-data-reader/json-data-reader.md) reads a JSON string as a forward-only sequence of rows, implementing the standard `IDataReader` interface so the result can be piped directly into any action that accepts a DataReader. [JsonDataReaderSchema](./json/json-data-reader/json-data-reader-schema.md) defines the mapping between JSON properties and target column names and data types.
