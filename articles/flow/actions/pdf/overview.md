# PDF overview

Flow includes built-in support for working with PDF files — converting them to Markdown for AI workflows, rendering them as images for OCR, or splitting a long document into smaller chunks. The actions in this category don't use a connection — they operate on PDF content passed in as a stream or byte array, typically read from a storage system like [OneDrive](../onedrive/overview.md) or [Azure Blob Storage](../azure-blob-storage/overview.md).

For more advanced PDF processing — such as converting PDFs with complex tables, images, or multi-column layouts to other formats — see the [Adobe](../adobe/overview.md) category, which uses the Adobe PDF Services API.

<br/>

## Explore

#### Converting PDFs to Markdown
[Convert a PDF file to Markdown](./convert-to-markdown.md) returns a Markdown representation of a PDF, primarily intended for AI workflows. The typical pipeline is reading a document from storage, converting to Markdown, splitting into chunks with [Split text](../ai/split-text.md), generating embeddings, and storing them in a vector database for [Retrieval-Augmented Generation (RAG)](../postgresql/vector-search.md). For PDFs with complex layouts (tables, images, multi-column), the Adobe-based [Convert a PDF file to a non-PDF file](../adobe/pdf-to-non-pdf-as-byte-array.md) action is recommended instead — it generally produces better results for non-trivial layouts.

<br/>

#### Converting PDFs to images
[Convert a PDF file to image](./convert-to-image.md) renders a PDF as PNG or JPG, primarily for cases where text extraction isn't practical and you need to process the document with OCR or a vision-capable AI model — for example, extracting structured data such as invoice amounts from scanned documents. By default the action returns a single image; with **Response mode** set to `Zip`, multi-page PDFs return a zip file containing one image per page. The result is a `ConversionResult` object with the file data and its media type (`image/png`, `image/jpg`, or `application/zip`).

<br/>

#### Splitting PDFs
[Split a PDF document](./split-document.md) divides a PDF into chunks of one or more pages, returning each chunk as a byte array. Use **Pages per chunk** to configure the chunk size. Often used together with subsequent processing steps (Markdown conversion, image rendering, or further conversions through the Adobe category) where smaller pieces are easier to handle than a large document.
