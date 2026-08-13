# Convert a PDF file to Markdown

Converts a PDF file to [Markdown](https://en.wikipedia.org/wiki/Markdown).


![Convert PDF To Markdown](../../../../images/flow/convert-PDF-to-markdown.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow reads a contract (PDF file) from [OneDrive](../onedrive/read-file-from-onedrive-as-byte-array.md), converts it to Markdown, [splits the text](../ai/split-text.md) into chunks, [generates embeddings](../azure-ai/generate-embedding.md) for each chunk, converts the generated vector into a SQL Server-compatible format and stores the text, vector, and document reference in a SQL Server table. This table can then be used for [vector search](../postgresql/vector-search.md) or to feed chat models with the extracted information. 

<br/>

## Properties

| Name                 | Required | Description                                                                                                   |
| -------------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| Title                | No | The title of the action.  |
| File data            | Yes | Specifies the source of the PDF file, which can be either a Stream or a Byte Array.   |
| Conversion engine    | Yes | Select the engine to use in conversion, see below for details. |
| Result variable name | Yes | The name of the variable in which the result will be stored. |
| Description          | No | Additional notes or comments about the action or configuration. |

<br/>

## Conversion engines

 - **Docling** provides the most comprehensive extraction and is best for complex PDFs (tables, mixed layouts, and images). It is the slowest option and can require significant resources for large documents. This is currently the only engine that supports image conversion.
 - **Kreuzberg** is a balanced option for large documents. It is typically faster and more resource-efficient than Docling, but may produce less accurate results in complex layouts.
 - **MarkItDown** is optimized for text-focused PDFs. It is a good choice when speed and clean text output are the priority, and it uses a custom fine-tuned model.

<br/>

## Returns

This action returns a string/text in Markdown format.

<br/>

>[!NOTE] 
> For best results when converting PDFs with complex content (e.g., tables, images, or multi-column layouts), it is recommended to use either [Adobe "Convert a PDF file to a non-PDF file"](../adobe/pdf-to-non-pdf-as-byte-array.md), or ["Convert to image"](./convert-to-image.md) instead of the "Convert a PDF file to Markdown" action.


