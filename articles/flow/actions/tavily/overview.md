# Tavily overview

Flow includes built-in support for [Tavily](https://docs.tavily.com/), a web search API designed for AI workflows. The category provides two ways to bring real-time web information into a flow: a direct **Web search** action you call yourself, and a **Web search tool** that an AI Agent can decide to call on its own. Common use cases include answering chat questions that depend on recent events, enriching stored data with web context, and giving AI agents access to current information beyond their training data.

To use any Tavily action, you first need a [Tavily connection](./connection.md) configured with an API key from the [Tavily dashboard](https://app.tavily.com/).

<br/>

## Explore

#### Connection
Set up the API key Flow uses to authenticate with Tavily. Required by both actions in this category.  
[Read more](./connection.md)

<br/>

#### Performing a web search
[Web search](./search-web.md) executes a Tavily search and returns a `WebSearchResult` containing an optional **Answer** (a summarized direct answer when available), a list of **Items** (each with Title, URL, Score, Snippet, and optional FullContent), plus `IsSuccess` and `ErrorMessage` fields. Common options control how many results to fetch (Max results), how much content per result (Max content length), whether to extract the full page content rather than just snippets, and an Include domains filter for restricting results to specific sites.

<br/>

#### Exposing web search to an AI Agent
[Web search tool](./web-search-tool.md) wraps the same web search functionality as a tool available to a [Tools AI Agent](../agents/tools-ai-agent.md) or directly to an LLM. The agent decides when to call it — for example when handling a user question that requires up-to-date information that wouldn't be in the model's training data. The configuration mirrors the direct Web search action (Max results, content length, Include answer, Extract full content, Include domains, request timeout) — the difference is who decides to invoke the search: in this case, the agent.
