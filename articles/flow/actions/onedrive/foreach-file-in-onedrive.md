# For each file in OneDrive

Iterates over all files in a OneDrive folder, one [OneDriveItem](./api-reference/onedrive-item.md) at a time.

![Onedrive Foreach File](../../../../images/flow/onedrive-foreach-file.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow iterates through all files in a OneDrive folder, deletes each file, and then sends an email notification once all deletions are completed.

## Properties

| Name              | Required | Description                                                                                                    |
| ----------------- | -------- | -------------------------------------------------------------------------------------------------------------- |
| Connection        | Yes | The connection to the OneDrive account. Setting up a connection requires a Microsoft `Work or School account`. |
| Folder            | No | The OneDrive folder to iterate through. If not specified, the root folder is used.                             |
| Drive item name   | Yes | The name of the variable that contains the current [OneDriveItem](./api-reference/onedrive-item.md) in each iteration. |
| Disabled          | No | If enabled, the node is skipped during execution.                                                              |
| Description       | No | Additional notes describing the purpose or behavior of this node.                                              |

<br/>

## Returns

A single [OneDriveItem](./api-reference/onedrive-item.md) containing metadata about the current file (or folder) in each iteration. The item is available in the variable specified by **Drive item name**.  
To get all items in a folder as a [List](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1) instead, use the [Get files in OneDrive](get-files-in-onedrive.md) action.
