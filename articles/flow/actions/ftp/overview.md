# FTP / SFTP overview

Flow includes built-in support for working with FTP and SFTP servers, allowing flows to upload, download, list, and delete files. A common pattern is moving files between systems — for example, downloading an invoice from an FTP server, converting it to a different format, and uploading the result back to a different folder, or fetching files from blob storage and depositing them on an FTP server for downstream consumption.

To use any FTP/SFTP action, you first need a [connection](./connecting-to-ftp.md) to the server. The connection supports both protocols — choose **FTP** or **SFTP** depending on your server, with default ports 21 (FTP) and 22 (SFTP). Authentication is via username and password, with an optional connection timeout.

<br/>

## Explore

#### Connection
Set up the connection used by every FTP/SFTP action. Configure host name, port, credentials, and protocol type.  
[Read more](./connecting-to-ftp.md)

<br/>

#### Listing files
Two actions return file metadata (name, size, modified date, and other properties wrapped in a `FileInfo` object) rather than just file names. [Get files info](./get-file-names.md) returns the contents of a directory as a list, and [For each file info](./foreach-file-name.md) iterates over them one at a time, with an option to include files from subdirectories. Useful for example when cleaning up files older than a given threshold based on modification date.

<br/>

#### Downloading files
Download a file's contents into a flow. [Download file as stream](./download-file-as-stream.md) returns the file as a stream, and [Download file as byte array](./download-file-as-byte-array.md) returns it as a byte array. Once downloaded, the contents are typically passed to a compatible action — for example, [CSV](../csv/overview.md) or [Excel](../excel/overview.md) loaders, or one of the [Adobe](../adobe/overview.md) PDF conversions — before the data can be used.

<br/>

#### Uploading and removing files
[Upload file](./upload-file.md) writes a file from any byte array or stream source into a target directory, with an option to overwrite if a file with the same name already exists. [Delete file](./delete-file.md) removes a single file by full path — often used together with For each file info to clean up old files based on a filter.
