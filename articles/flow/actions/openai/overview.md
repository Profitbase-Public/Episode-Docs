# OpenAI overview

Flow includes built-in support for [OpenAI](https://platform.openai.com/docs), letting you call GPT models directly from a flow to power chat completions, build AI agents, generate embeddings, and stream responses to interactive clients. Compared to other AI provider categories in Flow, OpenAI is the most complete — covering both chat models and embeddings, which together enable RAG (Retrieval-Augmented Generation) pipelines built entirely on OpenAI.

To use any OpenAI action, you first need an [OpenAI connection](./openai-connection.md) configured with an API key from the [OpenAI dashboard](https://platform.openai.com/account/api-keys). The same connection is reused across all actions.

<br/>

## Explore

#### Connection
Set up the API key Flow uses to authenticate with OpenAI. Required by every OpenAI action.  
[Read more](./openai-connection.md)

<br/>

#### Chat model for AI Agents
Defines an OpenAI model that an [AI Agent](../agents/overview.md) can use to reason about tasks and decide which tools to call. Use this when building tool-using agents rather than direct chat flows. Specify the model ID (for example `gpt-4` or `gpt-3.5-turbo`), temperature, and max tokens.  
[Read more](./agent-chat-model.md)

<br/>

#### Chat completion
Sends a prompt to an OpenAI model and returns the full response in one go. Supports a system prompt, conversation history, additional context (for RAG), and a prompt template with `@@context` and `@@userPrompt` placeholders. Optional grounding can be enabled to improve factual accuracy through web search.  
[Read more](./chat-completion.md)

<br/>

#### Streaming chat completion
Same configuration as Chat completion, but delivers the response incrementally as it is generated. Use this for interactive chat clients and assistant-like UIs where users should see output as it forms.  
[Read more](./streaming-chat-completion.md)

<br/>

#### Generating embeddings
Two actions produce embedding vectors from text — used to build RAG systems by indexing content as vectors and later finding semantically similar matches. [Generate embedding](./generate-embedding.md) returns a single embedding vector (`ReadOnlyMemory<float>`) for a given text input — use this when you want to write your own vector queries directly against PostgreSQL, Azure SQL, or another vector store. [Text embedder](./text-embedder.md) defines a reusable embedder object that can be plugged into AI nodes exposing a *Text embedder* port — useful when the same embedding configuration is shared across multiple steps in a pipeline. Both accept the embedding model ID, and Generate embedding additionally supports a configurable **Dimensions** value for models that allow it.
