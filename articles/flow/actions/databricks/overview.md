# Databricks overview

Flow includes built-in support for executing SQL statements against [Databricks](https://www.databricks.com/databricks-documentation), letting you query data from a Databricks workspace and use the results in the rest of your flow — for example, reading customer data from Databricks, transforming it, and inserting it into another database.

To use the Databricks action, you first need a [Databricks connection](./connecting-to-databricks.md) configured with the Base URL of your workspace and a Personal Access Token generated in your Databricks user settings.

<br/>

## Explore

#### Connection
Set up the connection used to authenticate against your Databricks workspace. Uses a Base URL and a Personal Access Token (PAT). The connection page also covers how to generate a token, recommended practices around scoping and rotation, and an optional API request timeout.  
[Read more](./connecting-to-databricks.md)

<br/>

#### Execute SQL statement returning data
Runs a SQL statement against a Databricks SQL Warehouse and iterates over the result in chunks — each chunk is exposed as a separate `IDataReader`, with a maximum size of 25 MB per chunk. Supports specifying a target catalog and schema, and accepts parameterized SQL. Use this action to read data from Databricks and feed it into other actions, such as inserting the rows into SQL Server or another destination.  
[Read more](./get-sql-execution-chunks.md)
