# Azure Files overview

Flow includes built-in support for working with [Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction) shares, allowing flows to upload, download, list, and delete files, manage directories, and retrieve metadata about share contents. A common pattern is moving files between storage systems — for example, transferring data from Azure Blob Storage to a file share so it can be consumed by applications expecting standard SMB-style file access.

To use any Azure Files action, you first need an [Azure Files connection](./connecting-to-azure-files.md). Flow supports two authentication types: a [Connection string with share name](./connecting-to-azure-files.md#connection-string-and-share-name) for broader access, or a [SAS URI for the share](./connecting-to-azure-files.md#sas-uri-for-share) for scoped, time-limited access. The connection can also be restricted to a specific root directory using the optional **Default Directory** property.

<br/>

## Explore

#### Connection
Set up the connection used by every Azure Files action. Choose between a connection string with share name, or a SAS URI pointing directly at a share. Optionally scope the connection to a specific root directory so that all actions using it are limited to that directory and its subfolders.  
[Read more](./connecting-to-azure-files.md)

<br/>

#### Listing share contents
[Get share items info](./get-share-items-info.md) retrieves a collection of files and directories from a share, with options to filter by prefix, include or exclude directories, and traverse subfolders. Each item returned includes metadata describing the file or directory.

<br/>

#### Reading files
Download a file's contents into a flow. [Read file as stream](./read-file-as-stream.md) returns the file as a stream, and [Read file as byte array](./read-file-as-byte-array.md) returns it as a byte array. Once read, the contents must be loaded with a compatible action — such as those in [Excel](../excel/overview.md), [CSV](../csv/overview.md), or [JSON](../json/overview.md) — before the data can be used.

<br/>

#### Writing and removing files
[Upload file](./upload-file.md) writes a file from a byte array or stream into a target directory, with an option to overwrite if the file already exists. [Delete file from a share](./delete-file.md) removes a single file by full path.

<br/>

#### Managing directories
[Create directory](./create-directory.md) creates a new directory in a share if it does not already exist. [Delete directory](./delete-directory.md) removes a directory, with an option to delete all of its contents in the same action. Both actions support **Raise exception on failure** to control whether errors halt the flow.
