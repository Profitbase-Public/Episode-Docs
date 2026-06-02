# For each row from query

Iterates over the rows returned by a SQL Server database query.



![Read Rows](https://profitbasedocs.blob.core.windows.net/flowimages/read-rows.png)

## Properties

| Name         | Type            | Description                                       |
|--------------|-----------------|---------------------------------------------------|
| Title           | No | The title or name of the command.     |
| Connection      | Yes | The [SQL Server Connection](./connection.md).         |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create Connection](./create-connection.md) action. |
| SQL expression and parameters   | Yes | The command to execute along with any parameters.   |
| Row to entity mapping | Yes | Mapping between column and entity property including data type.   |
| Row variable name | Yes | Name of Row variable.  |
| Row data type | No | Name of the created entity type.  |
| Command timeout (sec) | No | The time limit for command execution before it times out. Default is 120 seconds.|
| Description   | No | Additional notes or comments about the action or configuration. |

<br/>

[!INCLUDE [](__videos.md)]