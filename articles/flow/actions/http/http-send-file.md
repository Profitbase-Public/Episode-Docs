# HTTP send file

Sends (uploads) a file to an HTTP endpoint.

Use this action when you need to upload a file to an HTTP API or web endpoint — for example to submit a generated report, deliver an export to a third-party system, or pass a file to another service for processing. The action sends an HTTP `POST` request with the file as `multipart/form-data` content.

![HTTP Send File](/images/flow/http-send-file.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow uses **HTTP get file** to download a PDF invoice from a supplier's API, uses **HTTP send file** to convert it to an editable Word document, and [uploads](../azure-blob-storage/upload-blob.md) the result to Azure Blob Storage for further processing.

<br/>

## Properties

| Name                 | Required | Description |
|----------------------|----------|-------------|
| Title                | No  | The title of the action. |
| Authentication       | No  | The [authentication](./http-authentication.md) configuration to use if the endpoint requires it. Supports Basic Authentication, OAuth 2.0 Client Credentials and Bearer token. |
| File name            | Yes | Select or enter the name of the file, including the file extension, for example `report.csv`. |
| File data            | Yes | Select the source content for the file. It must be a byte array or a [Stream](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream). |
| URI                  | Yes | Select or enter the URI of the endpoint to send the file to. You can use Flow variables or [Workspace Variables](../../workspaces/workspace-variables.md) to build a dynamic URI. |
| Query parameters     | No  | Key/value pairs that are appended to the URI as a query string. |
| Headers              | No  | Key/value pairs that are sent as HTTP request headers. |
| File metadata        | No  | A JSON object with additional information about the file. See [File metadata](#file-metadata) below. |
| Content type         | No  | The [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) of the file, for example `application/pdf` or `text/csv`. If not specified, the content type is resolved from the file name extension. If it cannot be resolved, `application/octet-stream` is used. |
| Result variable name | No  | The name of the variable containing the HTTP response. Default: `response`. |
| Description          | No  | Additional notes or comments about the action or configuration. |

<br/>

## How the file is sent

The file is sent in a `POST` request with `multipart/form-data` content:

- The file is added as a form field named `file`, using the specified **File name** and **Content type**.
- If **File metadata** is defined, it is added as a second form field named `metadata`, with the file name `metadata.json` and content type `application/json`.

The receiving endpoint must accept `multipart/form-data` uploads and expect the file in the `file` field. Refer to the documentation of the API you are calling to check that this matches what it expects. If the API expects the raw file as the request body instead, use the [HTTP request](./http-request.md) action.

<br/>

## File metadata

Use **File metadata** to send additional information together with the file, such as a document type, a reference number, or a description. The metadata is defined as a JSON object, and you can combine hard-coded values and variables.

```json
{
  "documentType": "Invoice",
  "invoiceNumber": "10045",
  "customerId": "100198"
}
```

> [!NOTE]
> As when [building a JSON request body](./http-request.md#defining-the-request-body-by-building-a-json-object), you must remember to add quotes around `string` and `date` values.

<br/>

## Returns

Returns an [HttpResponse&lt;string&gt;](../../api-reference/built-in-types/http-response.md) object containing the response from the endpoint.

| Property        | Description |
|-----------------|-------------|
| IsSuccess       | `true` if the endpoint returned a success status code (200–299). |
| StatusCode      | The HTTP status code returned by the endpoint. |
| Headers         | The response headers. |
| ContentTypeName | The content type of the response, for example `application/json`. |
| Content         | The response body as a string. |

The action does not fail if the endpoint returns an error status code. Use an [If](../built-in/if.md) action to check the `IsSuccess` property, and for example [throw an exception](../built-in/throw-exception.md) if the upload failed.

<br/>

[!INCLUDE [](./__videos.md)]
