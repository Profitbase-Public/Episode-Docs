# Word overview

Flow includes built-in support for converting Word documents to Markdown through the [Convert a Word file to Markdown](./convert-to-markdown.md) action. The conversion is primarily intended for AI workflows — a typical pipeline reads a `.docx` file from a storage source such as [OneDrive](../onedrive/overview.md), converts it to Markdown, splits the result with [Split text](../ai/split-text.md), generates embeddings, and stores them in a vector database for [Retrieval-Augmented Generation (RAG)](../postgresql/vector-search.md).

The action accepts the source document as a stream or byte array, and returns a Markdown string ready for downstream processing or storage. It doesn't use a connection.

For converting other document formats to Markdown — PowerPoint, PDF, Excel, HTML, or web URLs — see the [Markdown](../markdown/overview.md) category, which collects all conversion actions in one place.
