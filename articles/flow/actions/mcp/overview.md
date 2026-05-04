# Model Context Protocol (MCP) overview

The **Model Context Protocol (MCP)** category lets Flow act as a client to external MCP servers, exposing the tools they offer to a [Tools AI Agent](../agents/tools-ai-agent.md). MCP is an open protocol for sharing tools and context with AI models — a Flow agent connected to MCP servers can use any tool those servers provide, on top of the [built-in agent tools](../agents/overview.md) such as OneDrive, Azure Blob Storage, or Markdown.

A typical use case is extending an AI agent with capabilities that aren't covered by Flow's native agent tools — for example, querying a database through an Azure SQL MCP server, or posting notifications through a Microsoft Teams MCP server, all from within the same agent and conversation.

<br/>

## Explore

#### Connection
[MCP client connection](./mcp-client-connection.md) defines how Flow reaches the remote MCP server. Specify the server endpoint, the **transport type** (SSE or Streamable HTTP — Flow uses AutoDetect by default), and optionally an authentication type if the server requires one. Refer to the documentation of the specific MCP server you're connecting to for the correct values.  
[Read more](./mcp-client-connection.md)

<br/>

#### Exposing MCP tools to an AI Agent
[MCP client tool](./mcp-client-tool.md) takes an MCP connection and makes the tools from that server available to a [Tools AI Agent](../agents/tools-ai-agent.md). You can either expose all tools the server offers, or select a subset.

> [!TIP]
> Even though you can expose every tool a server offers, it's worth being selective. LLMs can get confused when given too many tools to choose from — keeping the list focused on what the agent actually needs tends to produce better results.
