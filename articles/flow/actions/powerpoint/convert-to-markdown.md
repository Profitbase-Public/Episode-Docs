# Convert a PowerPoint file to Markdown

Converts a PowerPoint file to [Markdown](https://en.wikipedia.org/wiki/Markdown).

![Convert Power Point To Markdown](../../../../images/flow/convert-PowerPoint-to-markdown.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow reads a presentation (PowerPoint file) from [OneDrive](../onedrive/read-file-from-onedrive-as-byte-array.md), converts it to Markdown, [splits the text](../ai/split-text.md) into chunks, [generates embeddings](../azure-ai/generate-embedding.md) for each chunk, converts the generated vector into a SQL Server-compatible format and stores the text, vector, and document reference in a SQL Server table. This table can then be used for [vector search](../postgresql/vector-search.md) or to feed chat models with the extracted information.

<br/>

## Properties

| Name        | Required | Description                                                           |
| ------------| -------- | --------------------------------------------------------------------- |
| Title       | No  | The title of the action. |
| File data   | Yes | Specifies the source of the PowerPoint file, which can either be a Stream or a Byte Array. |
| Conversion engine    | Yes | Select the engine to use in conversion, see below for details. |
| Result variable name | Yes | The name of the variable in which the result will be stored. |
| Description          | No | Additional notes or comments about the action or configuration. |

<br/>

## Conversion engines

 - **Docling** provides the most complete extraction and is best for complex presentations, including tables, mixed slide layouts, and embedded images. It is the slowest option and can require significant resources for large files. This is currently the only engine that supports image conversion.
 - **Kreuzberg** is a balanced option for larger presentations. It is typically faster and more resource-efficient than Docling, but may produce less accurate results on complex slide layouts.
 - **MarkItDown** is optimized for text-focused presentations. It is a good choice when speed and clean text output are the top priority, and it uses a custom fine-tuned model.

<br/>

## Returns

This action returns a string/text in Markdown format.
