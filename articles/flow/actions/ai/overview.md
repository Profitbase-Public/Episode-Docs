# AI overview

The **AI** category contains general-purpose actions that support building advanced AI solutions in Flow. They handle the supporting work that AI workflows depend on: exposing flows as tools for [AI agents](../agents/overview.md), managing chat history within a model's context window, and preparing text for embedding and retrieval.

These actions are not tied to a specific AI provider. For provider-specific functionality, see [Azure AI](../azure-ai/overview.md), [OpenAI](../openai/overview.md), or [Anthropic AI](../anthropic/overview.md).

Use them together with a chat model and the actions in the [Agents](../agents/overview.md) category to build chat completion flows, tool-using agents, and RAG (Retrieval Augmented Generation) systems.

<br/>

## Explore

#### Flow AI tool
Expose a Flow as a tool that an AI agent can invoke to perform work on its behalf. Combined with the [Flow AI tool trigger](../../triggers/ai/flow-ai-tool-trigger.md), this lets you extend agents with any capability you can build in Flow.  
[Read more](./flow-ai-tool.md)

<br/>

#### Chat history truncation reducer
Trim a growing conversation to a target number of messages so the chat stays within the model's context window. Helps preserve recent context, avoid overload, and keep responses focused in long-running sessions.  
[Read more](./chat-history-truncation-reducer.md)

<br/>

#### Split text
Divide a large document into smaller chunks directly in a flow, ready to be passed to an embedding action and stored as vector records. Supports recursive character, token-based, and Markdown header splitting.  
[Read more](./split-text.md)

<br/>

#### Text splitter
Define a reusable `TextSplitter` object that can be plugged into AI nodes exposing a *Text splitter* port. Useful when the same splitting strategy is shared across multiple steps in a RAG pipeline.  
[Read more](./text-splitter.md)
