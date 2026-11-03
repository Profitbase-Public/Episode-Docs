# Truncate table

Truncates (empties) a table in a SQL Server database.

![SQL Example Truncate](https://profitbasedocs.blob.core.windows.net/flowimages/SQL-example-truncate.png)

## Properties

| Name         | Required | Description                                       |
|--------------|----------|---------------------------------------------------|
| Title        | No | A descriptive title for the action.               |
| Connection   | Yes | The [SQL Server Connection](./connection.md).         |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create SQL Server Connection](./create-connection.md) action. |
| Table name  | Yes | Enter or select the table name. |
| Command timeout | No | The time limit for command execution before it times out. Default is 120 seconds.|
| Description | No | Additional notes or comments about the action or configuration. |


![Truncate Table](https://profitbasedocs.blob.core.windows.net/flowimages/truncate-table.png)

<br/>

>[!NOTE]
> Selecting an existing **Table name** can only be used with a valid **Connection** and not **Dynamic connection**.

<br/>

[!INCLUDE [](__videos.md)]