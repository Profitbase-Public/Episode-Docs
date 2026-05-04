# OneDrive overview

Flow includes built-in support for working with Microsoft OneDrive, allowing flows to upload, download, list, and delete files in a user's OneDrive — and to expose OneDrive to AI agents as a tool. A typical pattern is reading files dropped into a OneDrive folder, processing them, and uploading the result back; or generating reports (Excel, PDF) from a database query and saving them to OneDrive for end users.

OneDrive actions don't use a shared connection page — the connection is configured per action and requires signing in with a **Microsoft Work or School account**. The actions then operate on that user's OneDrive, with files appearing as if uploaded or modified by the signed-in user.

<br/>

## Explore

#### Listing files
Two actions return information about the contents of a OneDrive folder. [Get files in OneDrive](./get-files-in-onedrive.md) returns a list of [OneDriveItem](./api-reference/onedrive-item.md) objects (id, name, path, size, last modified, media type, etc.) for the files in the folder, with an option to also include subfolders. [For each file in OneDrive](./foreach-file-in-onedrive.md) iterates the same items one at a time. The typical pattern is filtering items by name or extension and then reading the matching files.

<br/>

#### Reading files
Download a file's contents into a flow. [Read file from OneDrive as stream](./read-file-from-onedrive-as-stream.md) returns the file as a stream, and [Read file from OneDrive as byte array](./read-file-from-onedrive-as-byte-array.md) returns it as a byte array. Once read, the contents are typically passed to a compatible action — for example loading an Excel spreadsheet through [Read Excel file as DataTable](../excel/read-excel-file-as-datatable.md), or converting a PDF using one of the [Adobe](../adobe/overview.md) actions.

<br/>

#### Writing and removing files
[Upload file to OneDrive](./upload-file-to-onedrive.md) writes a file from a byte array or stream to a OneDrive folder. [Delete file from OneDrive](./delete-file-from-onedrive.md) removes a file by path — often used together with For each file in OneDrive to clean up after processing.

> [!NOTE]
> Upload file to OneDrive supports files up to **4 MB** in size, as defined by the underlying [Microsoft Graph API](https://learn.microsoft.com/en-us/onedrive/developer/rest-api/api/driveitem_put_content).

<br/>

#### Using OneDrive from an AI agent
[OneDrive Agent Tool](./agent-tool.md) lets a [Tools AI Agent](../agents/tools-ai-agent.md) work with OneDrive autonomously — listing files, reading their contents, and acting on them based on the agent's reasoning. A typical use is an agent that reads Word documents from a folder, summarizes each one, and emails the summaries to a recipient.
