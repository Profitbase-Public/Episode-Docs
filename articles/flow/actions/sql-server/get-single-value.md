# Get single value

Executes a SQL query against a SQL Server database and returns a single scalar value — the first column of the first row in the result set. The value is stored as a Flow variable with the type specified in **Result variable type**.

Use this when you need one value from the database — a count, a sum, a configuration setting, a serialized chat history, or any other single-cell result.

## When to use this

- To retrieve a scalar value such as a row count, sum, max, or configuration setting for use in downstream actions.
- To look up a single text value — for example, serialized chat history for an AI agent session.
- When you need one value from the database and a full entity or DataReader would be overhead.

## How it works

- **Input**: A SQL query (with optional parameters) and a SQL Server connection.
- **Processing**: Executes the query and reads the first column of the first row from the result set.
- **Output**: Returns the value as a Flow variable specified in **Result variable name**, cast to the type specified in **Result variable type**. If the query returns no rows, the variable is `null`.


<br/>

![Flow that provides chat completions with RAG using vector search, chat history retrieval via Get single value, streaming chat completion, and chat history persistence](../../../../images/flow/get-single-value.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow provides chat completions to a chat client with RAG and conversation memory. A Chat completion trigger receives the user message. [Search Vectors](../postgresql/vector-search.md) finds relevant context from a vector store. **Get chat history** retrieves the serialized conversation history for the current session from a SQL Server table using the `@SessionID` parameter. Streaming chat completion generates the response using the retrieved context and history, yielding messages back to the client. Save chat history persists the updated conversation. Use Get single value when you need to retrieve a single stored value — here, the chat history string — as input for a downstream action.

<br/>

## Properties

| Name         | Required | Description                                       |
|--------------|----------|---------------------------------------------------|
| **Title**              | No | A descriptive title for the action.               |
| **Connection**      | Yes | The [SQL Server Connection](./connection.md) to the target database.         |
| **Enable dynamic connection** | No | When enabled, uses a connection created at runtime by [Create Connection](./create-connection.md). Use this when the target database varies between runs. |
| **SQL expression and parameters**   | Yes | The SQL query to execute, with optional parameters. The query should return a single value (first column of the first row). See [Execute Command — How to use parameters](./execute-command.md#how-to-use-parameters) for details on the SQL expression editor. |
| **Result variable name** | Yes | The name of the Flow variable that holds the returned value. Default is `value`. |
| **Result variable type** | Yes | The .NET data type of the returned value (e.g. `String (Text)`, `Int 32`, `Decimal`). The query result is cast to this type. |
| **Command timeout (seconds)** | No | Maximum execution time in seconds. The action fails with a timeout error if exceeded. Default is 120 seconds. |
| **Disabled** | No | When checked, the action is skipped during Flow execution. |
| **Description**   | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

A single .NET value of the type specified by **Result variable type**. If the query returns no rows, the variable is `null`. If the query returns multiple rows or columns, only the first column of the first row is used.

<br/>

## How to use parameters

To use parameters in the query, declare and assign variables in the **Parameters** tab of the SQL expression editor. Then reference them in the query with the `@` prefix.

```sql
SELECT [Name] FROM Users WHERE UserId = @UserId
```

<br/>

## How to use Flow variables in the command expression

To use Flow variables in a SQL query as part of the expression, first [declare a variable](../built-in/declare-variable.md) as `Global` and [assign a value to the variable](../built-in/set-variable.md). Then enclose the variable name in curly brackets.

```sql
-- TableName is a Flow variable declared and assigned in a previous action.
SELECT [Name] FROM {TableName} WHERE UserId = @UserId
```

<br/>

## See also

- [Get Entity](./get-entity.md) — returns a single row as a typed entity object with multiple properties.
- [Get Entities](./get-entities.md) — returns multiple rows as a typed list.
- [Get DataReader](./get-datareader.md) — returns a forward-only stream for large result sets.
- [Execute Command](./execute-command.md) — executes a SQL command and returns the row count.
- [Connection](./connection.md) — how to set up a SQL Server connection.

<br/>

[!INCLUDE[](__videos.md)]