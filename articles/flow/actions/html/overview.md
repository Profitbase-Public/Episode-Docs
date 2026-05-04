# HTML overview

Flow includes built-in support for processing HTML documents — extracting specific elements and rewriting links — primarily for preparing web content to be stored in a vector database and used in Retrieval-Augmented Generation (RAG) for AI chats. A typical pipeline retrieves an HTML page, extracts only the relevant content (skipping menus, scripts, headers, and footers), rewrites relative links so they remain valid outside the original site, and converts the result to Markdown for embedding.

The actions in this category accept HTML as a string, byte array, or stream — typically the response from an [HTTP](../http/overview.md) request — and don't require a connection.

<br/>

## Explore

#### Extracting content
[For each element](./for-each-element.md) iterates over elements in an HTML document selected by CSS selectors. You provide the source HTML and a selector expression — for example `div.content` to grab all divs with the class `content`, or `header, article` to grab multiple element types — and the action returns each matching element as a string. The most common use is extracting the meaningful body of a page so that menus, navigation, and footers don't pollute the data being indexed.

<br/>

#### Fixing relative links
[Replace relative URLs](./replace-relative-urls.md) takes an HTML document or fragment along with a base URL, and rewrites all relative URLs into absolute ones. This is necessary when content is taken out of its original context — such as when chunks of a website are stored for RAG — because relative links would otherwise break once the AI surfaces them in a chat response. Often used immediately after [For each element](./for-each-element.md) on the extracted fragments before they're converted to Markdown and embedded.
