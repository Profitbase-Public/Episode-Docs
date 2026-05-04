# Anthropic AI overview

Flow includes built-in support for [Anthropic](https://docs.claude.com/en/api/overview) models, letting you call Claude directly from a flow to power chat completions, build AI agents, and stream responses to interactive clients.

To use any Anthropic action, you first need an [Anthropic connection](./anthropic-connection.md) with an API key generated in the [Claude Console](https://platform.anthropic.com/account/api-keys). The same connection is reused across all actions.

<br/>

## Explore

#### Connection
Set up the API key Flow uses to authenticate with Anthropic. Required by every Anthropic action.  
[Read more](./anthropic-connection.md)

<br/>

#### Chat model for AI Agents
Defines an Anthropic model that an [AI Agent](../agents/overview.md) can use to reason about tasks and decide which tools to call. Use this when building tool-using agents rather than direct chat flows.  
[Read more](./agent-chat-model.md)

<br/>

#### Chat completion
Sends a prompt to Claude and returns the full response in one go. Supports a system prompt, conversation history, additional context (for RAG), and a prompt template with `@@context` and `@@userPrompt` placeholders. Optional grounding and web fetch can be enabled to improve factual accuracy.  
[Read more](./chat-completion.md)

<br/>

#### Streaming chat completion
Same configuration as Chat completion, but delivers the response incrementally as it is generated. Use this for interactive chat clients and assistant-like UIs where users should see output as it forms.  
[Read more](./streaming-chat-completion.md)
