# Adobe overview

Flow includes built-in support for the [Adobe PDF Services API](https://developer.adobe.com/document-services/docs/overview/) for working with PDF files — converting between PDF and other document formats (Word, PowerPoint, Excel, RTF), generating PDFs from non-PDF sources, and extracting structured content from existing PDFs. Use this category when you need higher-quality conversion than the built-in [PDF](../pdf/overview.md) actions can provide — particularly for documents with complex layouts, tables, images, or multi-column content.

To use any Adobe action, you first need an [Adobe connection](./adobe-connection.md) configured with a Client ID and Client Secret obtained from the Adobe Developer Console.

<br/>

## Explore

#### Connection
Set up the Adobe connection used by every action in this category. Requires API credentials created in the Adobe Developer Console — the connection page covers the credential setup steps.  
[Read more](./adobe-connection.md)

<br/>

#### Converting PDF to other formats
[Convert PDF file to non-PDF file as byte array](./pdf-to-non-pdf-as-byte-array.md) and [Convert PDF file to non-PDF file as stream](./pdf-to-non-pdf-as-stream.md) take a PDF and convert it to a target format — DOC, DOCX, PPTX, RTF, or XLSX — returning the result as either a byte array or a stream depending on what fits the downstream use. The typical pattern is reading a PDF from storage (such as [OneDrive](../onedrive/overview.md)), converting it, and uploading the result back.

<br/>

#### Generating PDFs from other formats
[Convert non-PDF file to PDF file as byte array](./non-pdf-to-pdf-as-byte-array.md) and [Convert non-PDF file to PDF file as stream](./non-pdf-to-pdf-as-stream.md) do the reverse — take a Word document, image, PowerPoint, or similar source and produce a PDF. Used when a flow needs to standardize incoming files into PDF for archiving, downstream processing, or delivery.

<br/>

#### Extracting content from PDFs
Four actions extract the content of a PDF in different shapes, depending on what you need to do with it. [Extract content from PDF as byte array](./extract-content-from-pdf-as-byte-array.md) and [Extract content from PDF as stream](./extract-content-from-pdf-as-stream.md) return the extracted output as raw bytes or a stream — useful when downstream actions consume files directly. [Extract content from PDF as JSON](./extract-content-from-pdf-as-json.md) returns a JSON string with three available output shapes: `JSON_Raw` (full layout metadata including positions and fonts), `JSON_Simplified`, and `JSON_Hierarchical` — pick the level of detail that matches whether you need exact layout reproduction or a cleaner structural view. [Extract content from PDF as document tree](./extract-content-from-pdf-as-object-tree.md) returns a [PdfTree](./apis/pdf-tree.md) that you can traverse programmatically inside a [Function](../built-in/function.md) — useful for cases like extracting specific fields from invoices or product information from structured documents.

<br/>

[!INCLUDE [](./__videos.md)]
