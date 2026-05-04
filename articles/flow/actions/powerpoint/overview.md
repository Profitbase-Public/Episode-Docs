# PowerPoint overview

Flow includes built-in support for converting PowerPoint files to Markdown through the [Convert a PowerPoint file to Markdown](./convert-to-markdown.md) action. The conversion is primarily intended for AI workflows — a typical pipeline reads a `.pptx` file from a storage source such as [OneDrive](../onedrive/overview.md), converts it to Markdown, splits the result with [Split text](../ai/split-text.md), generates embeddings, and stores them in a vector database for [Retrieval-Augmented Generation (RAG)](../postgresql/vector-search.md).

The action accepts the source presentation as a stream or byte array, and returns a Markdown string ready for downstream processing or storage. It doesn't use a connection.

For converting other document formats to Markdown — Word, PDF, Excel, HTML, or web URLs — see the [Markdown](../markdown/overview.md) category, which collects all conversion actions in one place.
