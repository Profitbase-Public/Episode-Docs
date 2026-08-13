# Convert a URL address to Markdown

>[!NOTE]
> This action will be removed from Hypergene Flow starting with version 1.14 (August 2026).


Returns [Markdown](https://en.wikipedia.org/wiki/Markdown) text from a URL (HTML page address).


![Convert URL To Markdown](../../../../images/flow/convert-URL-to-markdown.png)

**Example** ![Example](../../../../images/strz.jpg)  
This Flow reads all URLs from a [website sitemap](get-sitemap.md), converts the HTML of [each](../built-in/foreach.md) page to Markdown, [splits the content](../ai/split-text.md) into manageable chunks, [generates embeddings](../azure-ai/generate-embedding.md) for each chunk, converts the generated vector into a SQL Server-compatible format and stores the text, vector, and document reference in a SQL Server table. This table can then be used for [vector search](../postgresql/vector-search.md) or to feed chat models with the extracted information.

<br/>

## Properties

| Name                 | Required | Description                                                       |
| -------------------- | -------- | --------------------------------------------------- |
| Title                | No |   The title of the action.                    |
| URL                  | Yes | Specifies the addresse to a web page.                        |
| Result variable name | Yes | The name of the variable in which the result will be stored.  |
| Description          | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

This action returns a string/text in Markdown format.

<br/>

[!INCLUDE [](./__videos.md)]