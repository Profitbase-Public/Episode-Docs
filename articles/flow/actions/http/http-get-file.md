# HTTP get file

Downloads a file from an HTTP endpoint into memory as a byte array.

Use this action when you need to fetch a file — for example a PDF, image, Excel workbook or CSV export — from a URL or an HTTP API, so it can be processed or stored by the rest of the Flow. The action sends an HTTP `GET` request to the specified URI and returns the raw response body.

![HTTP Get File](/images/flow/http-get-file.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow uses **HTTP get file** to download a PDF invoice from a supplier's API, [converts](../adobe/pdf-to-non-pdf-as-byte-array.md) it to an editable Word document, and [uploads](../azure-blob-storage/upload-blob.md) the result to Azure Blob Storage for further processing.

<br/>

## Properties

| Name                 | Required | Description |
|----------------------|----------|-------------|
| Title                | No  | The title of the action. |
| Authentication       | No  | The [authentication](./http-authentication.md) configuration to use if the endpoint requires it. Supports Basic Authentication, OAuth 2.0 Client Credentials and Bearer token. |
| URI                  | Yes | Select or enter the URI of the file to download. You can use Flow variables or [Workspace Variables](../../workspaces/workspace-variables.md) to build a dynamic URI. |
| Query parameters     | No  | Key/value pairs that are appended to the URI as a query string, for example `format=pdf`. |
| Headers              | No  | Key/value pairs that are sent as HTTP request headers, for example `Accept: application/pdf`. |
| Return variable name | Yes | The name of the variable containing the downloaded file. Default: `fileData`. |
| Description          | No  | Additional notes or comments about the action or configuration. |

<br/>

## Returns

Returns the response body as a [byte](https://learn.microsoft.com/en-us/dotnet/api/system.byte) [array](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/arrays).

The byte array can be passed directly to any action that accepts file data, such as [Upload Blob](../azure-blob-storage/upload-blob.md), [Upload file](../ftp/upload-file.md), or [HTTP send file](./http-send-file.md).

> [!NOTE]
> This action returns only the file contents. If you need access to the HTTP status code or response headers, for example to read the file name from the `Content-Disposition` header, use the [HTTP request](./http-request.md) action with the response content type set to `Byte array` instead.

<br/>

[!INCLUDE [](./__videos.md)]
