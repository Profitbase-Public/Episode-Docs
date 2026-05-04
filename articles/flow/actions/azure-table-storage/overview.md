# Azure Table Storage overview

Flow includes built-in support for working with [Azure Table Storage](https://learn.microsoft.com/en-us/azure/storage/tables/table-storage-overview), allowing flows to add, read, update, and delete entities in a table — for example, maintaining a list of employees and updating their status based on activity, or removing records that haven't been touched in months.

To use any Table Storage action, you first need an [Azure Table Storage connection](./connecting-to-azure-table-storage.md). Flow supports four authentication types: a SAS URI scoped to a specific table, a SAS URI scoped to the whole storage account, a connection string for the storage account, or a connection string combined with a table name to lock the connection to a single table. Every entity in Table Storage is identified by a **PartitionKey** and a **RowKey**, which most actions in this category require you to specify.

<br/>

## Explore

#### Connection
Set up the connection used by every Table Storage action. Choose one of four authentication options depending on how broad the access should be — from a SAS URI scoped to a single table (most restricted) to a connection string with full account access. The connection page also covers how to generate SAS URIs and which permissions to enable for read, add, update, and delete.  
[Read more](./connecting-to-azure-table-storage.md)

<br/>

#### Reading entities
[For each Entity](./foreach-table-entity.md) iterates over entities in a table, with optional simple or advanced filtering to narrow down which entities are processed. Use this to scan a table or process matching records one by one — for example, finding all employees inactive for more than 30 days.

<br/>

#### Adding entities
[Add Entity](./add-table-entity.md) inserts a new entity into a table. The PartitionKey can be derived from an entity property or set to a custom value, and **Overwrite existing** controls whether an existing entity with the same keys is replaced or causes the action to fail.

<br/>

#### Updating entities
[Update Entity](./update-table-entity.md) modifies an existing entity, with two update modes: **Merge** keeps properties that aren't part of the update, while **Replace** removes them. PartitionKey handling works the same way as in Add Entity.

<br/>

#### Deleting entities
[Delete Entity](./delete-table-entity.md) removes a single entity identified by its PartitionKey and RowKey. Often used together with For each Entity to clean up records based on a filter — for example, deleting employees who have been inactive for more than 12 months.
