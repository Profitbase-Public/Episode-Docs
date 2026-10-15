# Return File HTTP response

Terminates the Flow and returns a file as the HTTP response to the caller.

Use this action when a client calls the Flow over HTTP and expects a file in return. Typical use cases are 3rd party applications or [InVision Workbooks](../../../invision/docs/flows/how-to/download-file-from-flow.md) that call a Flow to generate or fetch a file on demand, for example a PDF report or an Excel export.

![Return File HTTP Response](/images/flow/http-return-file-response.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow generates an HTML report for a given portfolio and returns it as a PDF file. It uses the portfolio ID, report ID and response format to generate the HTML report data, converts that data to the type required by the next step, converts the HTML content into a PDF byte array, and uses **Return File HTTP response** to return the PDF file to the caller.

<br/>

## Properties

| Name                | Required | Description |
|---------------------|----------|-------------|
| Title               | No  | The title of the action. |
| File contents       | Yes | Select the variable containing the file to return, for example a byte array. |
| File name           | No  | Select or enter the name of the file, including the file extension, for example `report.pdf`. The extension is used to determine the content type of the response. Default: `ReturnFileHttpResponse`. |
| Content disposition | No  | Specifies how the client should handle the file. See [Content disposition](#content-disposition) below. |
| Description         | No  | Additional notes or comments about the action or configuration. |

<br/>

## Content disposition

The [Content-Disposition](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition) header tells the client whether the file should be displayed or downloaded.

| Value        | Description |
|--------------|-------------|
| *(empty)*    | The file is returned as a download, using the specified **File name**. This is the default. |
| `attachment` | The client should download the file and save it using the specified **File name**. |
| `inline`     | The client should display the file directly, if possible, for example by showing a PDF in the browser. |

The **File name** is always added to the `Content-Disposition` header, so you should not include it in the **Content disposition** value.

<br/>

## Content type

The content type of the response is determined from the extension of the **File name**, for example `application/pdf` for `.pdf` and `text/csv` for `.csv`. If no file name is specified, or the extension is unknown, the content type is set to `application/octet-stream`.

<br/>

## Remarks

- This action terminates the Flow, in the same way as the [Return](../built-in/return.md) action. It has no outgoing execution port, so no actions can run after it.
- Only **short-running** Flows can return a file directly to the caller. See [Long vs short running Flows](../../flows/long-vs-short-running-flows.md) for more information.
- To call the Flow from an InVision Workbook and download the file, see [Download a file from a Flow](../../../invision/docs/flows/how-to/download-file-from-flow.md).

<br/>

[!INCLUDE [](./__videos.md)]
