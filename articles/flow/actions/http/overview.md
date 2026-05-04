# HTTP overview

Flow includes built-in support for making generic HTTP requests, exchanging files over HTTP, and returning files in response to incoming HTTP calls — together with helpers for working with web content (sitemaps, HTML to Markdown conversion). Use these actions whenever you need to integrate with an HTTP API that doesn't have its own dedicated category, fetch resources from the web, or expose a flow as an HTTP-callable file generator.

The actions in this category don't use a shared connection — authentication is configured per request, with [Basic Authentication and OAuth 2.0 Client Credentials](./http-authentication.md) supported out of the box.

<br/>

## Explore

#### Authentication
[HTTP Authentication](./http-authentication.md) describes the supported authentication types — Basic Authentication for username/password, and OAuth 2.0 Client Credentials for server-to-server flows that exchange a client ID and secret for an access token. The same configuration is reused across the request actions below.

<br/>

#### Generic HTTP requests
[HTTP request](./http-request.md) calls any HTTP API to run an operation or fetch data. Supports `GET`, `POST`, `PUT`, `DELETE`, dynamic URLs built from Flow or workspace variables, query parameters, headers, and request bodies defined either as a JSON object or from a variable. Use this whenever there is no dedicated category for the API you're integrating with.

<br/>

#### Exchanging files over HTTP
[HTTP get file](./http-get-file.md) downloads a file from an HTTP endpoint into a flow, and [HTTP send file](./http-send-file.md) uploads a file to one. Both support query parameters and headers for cases where the endpoint requires them.

<br/>

#### Returning files from a flow
[Return File HTTP response](./return-file-http-response.md) returns a file as the HTTP response from a flow — typically used when a third-party application calls the flow over HTTP and expects a file back, such as generating a PDF report on demand and streaming it to the caller.

<br/>

#### Working with web content
Three actions help when the source is a website rather than an API. [Get sitemap](./get-sitemap.md) fetches the `sitemap.xml` of a URL — useful as a starting point for scraping or indexing a site. [Convert a URL to Markdown](./convert-url-to-markdown.md) downloads a page and converts it directly to Markdown in one step. [Convert HTML to Markdown](./convert-html-to-markdown.md) does the same conversion when you already have the HTML in hand (for example after fetching it via HTTP request and processing it with the [HTML](../html/overview.md) actions). The typical pipeline is: read a sitemap, iterate over its URLs, convert each page to Markdown, chunk it, and store embeddings for use in RAG.

<br/>

[!INCLUDE [](./__videos.md)]
