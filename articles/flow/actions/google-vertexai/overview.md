# Google Vertex AI overview

Flow includes built-in support for [Google Vertex AI](https://cloud.google.com/vertex-ai), letting you call Gemini and other Vertex AI models directly from a flow to power chat completions, build AI agents, and stream responses to interactive clients.

To use any Vertex AI action, you first need a [Vertex AI connection](./vertexai-connection.md) configured with a service account JSON key from the [Google Cloud Console](https://console.cloud.google.com/) and the region where your model runs. Setting up the connection requires a Google Cloud project with billing enabled, the Vertex AI API enabled, and a service account assigned the **Vertex AI User** role — the connection page covers each step.

<br/>

## Explore

#### Connection
Set up the JSON key and region Flow uses to authenticate with Vertex AI. Required by every Vertex AI action.  
[Read more](./vertexai-connection.md)

<br/>

#### Chat model for AI Agents
Defines a Vertex AI model that an [AI Agent](../agents/overview.md) can use to reason about tasks and decide which tools to call. Use this when building tool-using agents rather than direct chat flows. Specify the model ID (for example `gemini-2.5-pro` or `gemini-2.0-flash-lite-001`), temperature, and max tokens.  
[Read more](./agent-chat-model.md)

<br/>

#### Chat completion
Sends a prompt to a Vertex AI model and returns the full response in one go. Supports a system prompt, conversation history, additional context (for RAG), and a prompt template with `@@context` and `@@userPrompt` placeholders. Optional grounding can be enabled to improve factual accuracy through web search.  
[Read more](./chat-completion.md)

<br/>

#### Streaming chat completion
Same configuration as Chat completion, but delivers the response incrementally as it is generated. Use this for interactive chat clients and assistant-like UIs where users should see output as it forms.  
[Read more](./streaming-chat-completion.md)
