# Convert HTML to Markdown

Returns [Markdown](https://en.wikipedia.org/wiki/Markdown) text from a HTML document (page).


![Convert HTML To Markdown](../../../../images/flow/convert-HTML-to-markdown.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow downloads the [HTML](http-request.md) of a specified page, converts the HTML to Markdown, [splits the cleaned content](../ai/split-text.md) into smaller chunks, [generates embeddings](../azure-ai/generate-embedding.md) for each chunk, converts the embeddings into a SQL Server–compatible format, and stores the text, vector, and document reference in a SQL Server table. This table can then be used for [vector search](../postgresql/vector-search.md) or to feed chat models with the extracted information.

<br/>

## Properties

| Name                 | Required | Description                             |
| -------------------- | -------- | --------------------------------------- |
| Title                | No | The title of the action.                    |
| File data            | Yes | Specifies the source of the HTML text, which can either be a string, a Stream or a Byte Array. |
| Conversion engine    | Yes | Select the engine to use in conversion, see below for details. |
| Result variable name | Yes | The name of the variable in which the result will be stored.  |
| Description          | No | Additional notes or comments about the action or configuration. |

<br/>

## Conversion engines

 - **Docling** provides the most complete extraction and is best for complex HTML pages, including nested structures, rich formatting, tables, and image-related content. It is the slowest option and can require significant resources for large pages. This is currently the only engine that supports image conversion.
 - **Kreuzberg** is a balanced option for larger or high-volume HTML conversions. It is typically faster and more resource-efficient than Docling, but may produce less accurate results on complex or deeply nested layouts.
 - **MarkItDown** is optimized for text-focused HTML content. It is a good choice when speed and clean text output are the top priority, and it uses a custom fine-tuned model.

<br/>

## Returns

This action returns a string/text in Markdown format.

<br/>

[!INCLUDE [](./__videos.md)]