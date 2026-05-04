# Chat model

This defines a [Google VertexAI](https://cloud.google.com/vertex-ai) chat model that an AI Agent — such as the Tools AI Agent — can use to understand context, interpret user input, and decide which actions to take. By providing this model, the agent gains the ability to reason, plan, and respond intelligently based on the conversation and available tools.


![Vertexai Chat Model](../../../../images/flow/vertexai-chat-model.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow uses an AI Agent powered by a Google Vertex AI chat model to retrieve a list of blobs from Azure Blob Storage and store the results in a table.

## Properties

| Name                  | Required | Description |
|-----------------------|-----------|-------------|
| Title                 | No | The title of the model. |
| Connection            | Yes | Defines the [connection](vertexai-connection.md) to **VertexAI**. |
| Model Id       | Yes |	The identifier of the model to be used, such as gemini-2.5-pro, gemini-2.0-flash-lite-001, etc. This determines the capabilities and cost of the model. |
| Temperature       | No |Controls the variability and creativity of generated text. Accepts values from 0.0 to 2.0 (default 1.0). Lower values result in focused, predictable output, while higher values (e.g., 1.5) produce more diverse and creative responses. |
| Max Tokens | No | Sets a limit on the number of tokens (words, characters, or pieces of text) in the model’s response. |
| Result variable name | No | The name of the variable in which the result will be stored. |
| Description       | No | Additional details or notes. |

<br/>