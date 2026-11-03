# Update entity

Updates a table in SQL Server using data from an entity.  

At least one `Update key` is required. If you don't specify an Update key, the action will fail.  


Logically, this action performs the following:

```sql
UPDATE tableName
SET tableName.X = entity.X, tableName.Y = entity.Y
WHERE tableName.Key1 = entity.Key1 AND tableName.Key2 = entity.Key2
```

All properties in the `Update keys` configuration will be used in the WHERE clause to identify which row to update.  
All properties _NOT_ in the `Update keys` configuration will be used in the SET clause to identify which columns to update with data from the entity.

> [!NOTE]
> Each property name in the entity must exist in the target table. If the database collation is case sensitive, the properties and columns must match by case in addition to name.



![Update Entity](https://profitbasedocs.blob.core.windows.net/flowimages/update-entity.png)

## Properties

| Name         | Required | Description                                       |
|--------------|----------|---------------------------------------------------|
| Title        | No | A descriptive title for the action.               |
| Connection   | Yes | The [SQL Server Connection](./connection.md).         |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create SQL Server Connection](./create-connection.md) action. |
| Source   | Yes | Select the entity to use in update. |
| Table name | Yes | Select or enter the table name to insert into. |
| Update key | Yes | Select the table columns to use for update keys.  |
| Result variable name | No | Name of resulting list of entities variable  |
| Command timeout | No | The time limit for command execution before it times out. Default is 120 seconds.|
| Description   | No | Additional notes or comments about the action or configuration. |

<br/>

>[!NOTE]
> Selecting an existing **Table name** can only be used with a valid **Connection** and not **Dynamic connection**.

<br/>

[!INCLUDE [](__videos.md)]