# Markdown overview

The **Markdown** category groups together all actions that convert content from other formats into [Markdown](https://en.wikipedia.org/wiki/Markdown). The actions themselves live in their respective format categories (Excel, PDF, Word, etc.) — this category collects them in one place because they share a common purpose: turning structured documents into LLM-friendly plain text.

The most common use case is preparing documents for AI workflows. A typical pipeline reads a document from a file source, converts it to Markdown, splits the text into chunks with [Split text](../ai/split-text.md), generates embeddings, and stores the chunks in a vector database for [Retrieval-Augmented Generation (RAG)](../postgresql/vector-search.md) or to feed chat models with the document's content.

<br/>

## Explore

#### Converting documents to Markdown
Six actions cover the supported source formats:

- [Convert a Word file to Markdown](../word/convert-to-markdown.md) — for `.docx` documents
- [Convert a PDF file to Markdown](../pdf/convert-to-markdown.md) — for PDF files
- [Convert a PowerPoint file to Markdown](../powerpoint/convert-to-markdown.md) — for `.pptx` presentations
- [Convert an Excel file to Markdown](../excel/convert-to-markdown.md) — for `.xlsx` spreadsheets
- [Convert HTML to Markdown](../http/convert-html-to-markdown.md) — when you already have an HTML string, byte array, or stream
- [Convert a URL address to Markdown](../http/convert-url-to-markdown.md) — fetches a web page and converts it in one step

All six produce Markdown text as output, which can then be passed to downstream AI actions or stored as a string in a database or file.

<br/>

#### Exposing Markdown conversion to an AI Agent
[Markdown Agent Tool](./agent-tool.md) makes the same conversion functionality available to a [Tools AI Agent](../agents/tools-ai-agent.md), so the agent can reason about the contents of files it encounters — for example, an agent that reads Word documents from OneDrive, summarizes them, and emails the summaries can use this tool to convert each document into a format the chat model can read.
