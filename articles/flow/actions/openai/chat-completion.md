# Chat Completion

This defines an [OpenAI](https://platform.openai.com/docs/api-reference/introduction) chat completion model that processes a prompt, understands the user’s intent, and generates the next response. Using chat completion provides structured reasoning, allows the model to follow context, and helps maintain a coherent dialogue.   

This action is typically used in flows where you need the model’s complete output in a single, finalized response instead of receiving partial tokens over time.
Unlike the [streaming](streaming-chat-completion.md) version, this action delivers the **complete output in one response**.


![Open AI Chat Completion](/images/flow/openAI-chat-completion.png)  

**Example** ![Example](../../../../images/strz.jpg)  
This flow processes a user's chat question by first receiving it through a [Chat completion trigger](../../triggers/ai/chat-completion-trigger.md), then converting it into a vector using a [Text embedder](text-embedder.md), performing a [Vector search](../postgresql/vector-search.md) in a PostgreSQL database for relevant context, and finally passing the user input and retrieved context to OpenAI **Chat completion**, which generates a response that is returned to the client via the [Return](../built-in/return.md) node.


## Properties

| Name                  | Required | Description |
|-----------------------|-----------|-------------|
| Title             | No | The title of the action. |
| Connection        | Yes | Defines the [connection](openai-connection.md) to **OpenAI**. |
| Model id   | Yes | Id of the chosen [Model](https://platform.openai.com/docs/models).|
| User Prompt       | Yes | The input message from the user, which the model processes to generate a response. |
| System Prompt     | No | A system-level instruction that guides the model’s behavior and response style. |
| History           | No | A record of past interactions that provides context to the conversation, helping the model maintain continuity. |
| Context               | No | Additional context information to influence the response (if supported). |
| Prompt template   | No | Defines the structure of the prompt sent to the model. The system replaces the placeholders @@context and @@userPrompt with the relevant information. See example below. |
| Temperature       | No |Temperature in models controls the randomness and creativity of the generated responses. Lower temperatures (e.g., 0.2) produce more focused, predictable text, ideal for tasks that require precision. Higher temperatures (e.g., 1.5) increase creativity and variability, but may risk generating less coherent or relevant content, making it important to adjust based on your desired outcome. The default is 0.7 if nothing is defined by the user.|
| Max Completion Tokens | No | Sets a limit on the number of tokens (words, characters, or pieces of text) in the model’s response. |
| Result Variable Name | No | Stores the generated AI response. Default: "response". |
| Enable Grounding        | No | Enables web search to improve factual reliability by using external or structured context sources. |
| Disabled                | No | If enabled, the action will be skipped during flow execution. |
| Description       | No | Additional details or notes regarding the chat completion setup. |

<br/>

#### Prompt template

The prompt template allows you to specify the format of the prompt that is sent to the language model. This is useful for customizing how context and instructions are provided to the model.
Within the template, you can use the following placeholders:

- `@@context`: This is replaced by the "Context" property value.
- `@@userPrompt`: This is replaced by the "User prompt" property value.

The system will substitute these placeholders with the corresponding values before sending the prompt to the model.

**Example** ![Example](../../../../images/strz2.jpg) 

![Openai Chat Completion Prompt Template](../../../../images/flow/openai-chat-completion-prompt-template.png)  
