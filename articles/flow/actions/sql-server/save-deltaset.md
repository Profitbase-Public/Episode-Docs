# Save DeltaSet

Saves all changes in a DeltaSet to an Azure SQL or SQL Server table.  

A typical use case for this action is to save the changes made by users in the [Writeback Table](../../../PowerBI/writeback-table/overview.md) or [Writeback Comments](../../../PowerBI/writeback-comments/overview.md) visuals for Power BI. This action will save all changes made by the user in Power BI by either inserting, updating, or deleting rows in the SQL Server / Azure SQL target table.

![SQL Server Save DeltaSet](/images/flow/sqlserver-save-deltaset.png)

## Properties
| Name         | Required | Description                                 |
|--------------|-----------|---------------------------------------------|
| Title        | No | A descriptive title of the action.          |
| Connection      | Yes | The [SQL Server Connection](./connection.md).         |
| Dynamic connection | No | Use this option if you need to use a connection created by the [Create SQL Server Connection](./create-connection.md) action. |
| DeltaSet     | Yes | The DeltaSet to save to the target table.   |
| Target table | Yes | The name of the table where data will be stored. If this differs from the table the data is read from in the Power BI model, ensure the target table has columns with matching names and data types as defined in the column definitions of the Writeback Table visual. |
| [Save data options](#save-data-options) | No | For advanced scenarios (see below), use this option to override the default behavior for applying DeltaSet changes.  |  
| Command timeout | No | The time limit in seconds for command execution before it times out. Default is 120 seconds.|

<br/>

### Save data options

By default, the columns or fields you have defined as keys in the Power BI visual(s) will be used to determine which rows to update, delete or insert. This information is contained in the [DeltaSet](../../api-reference/built-in-types/deltaset.md). For example, if you have used DepartmentID and Period as key columns in the Power BI Comments visual, Flow will match on the DepartmentID and Period column in the target table to determine whether or not to do an update, insert or delete on that dimensionality. If, for some reason, you want to override which columns are used as qualifiers to save changes, you can use the `Save data options` configuration to handle this.

![DeltaSet metadata options](/images/flow/deltaset-metadata-options.png)

| Property                 | Description                          |
|--------------------------|--------------------------------------|
| Column name              | The name of the column that you want to specify a behavior for.  |
| Use as update or delete key | By enabling this option, Flow will match by this column when updating or deleting rows instead of using the columns defined as keys in the [DeltaCells](../../api-reference/built-in-types/deltaset.md#deltacell).  |
| Allow updating data in this column | You can set this option to `false` to explicitly prevent this column from being updated, even though the DeltaSet contains changes for the column.   |
| Enable identity insert  | Enable this option if you need to enable inserting data to an identity column. In most databases, if you try to insert data to an identity column, the operation will fail unless explicitly enabled. |


<br/>

>[!NOTE]
> Selecting an existing **Target table** can only be used with a valid **Connection** and not **Dynamic connection**.

<br/>


[!INCLUDE [](__videos.md)]