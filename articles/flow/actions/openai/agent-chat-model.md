# OpenAI chat model

Defines an [OpenAI](https://platform.openai.com/docs) chat model that an AI Agent, such as the Tools AI Agent, can use to reason about what actions to take.

![Open Aiagent Chat Model](../../../../images/flow/OpenAIagent-chat-model.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow uses an AI Agent powered by an OpenAI chat model to retrieve a list of blobs from Azure Blob Storage and store the results in a table.



## Properties

| Name                  | Required  | Description |
|-----------------------|-----------|-------------|
| Title                 | No        | The title of the action. |
| Connection            | Yes       | Defines the [connection](openai-connection.md) to **OpenAI**. |
| Model Id              | Yes       | The id of the model to be used, such as `gpt-5.5`, `gpt-5.6`, etc. This determines the capabilities and cost of the model. |
| Temperature           | No        |Temperature in models controls the randomness and creativity of the generated responses. Lower temperatures (e.g., 0.2) produce more focused, predictable text, ideal for tasks that require precision. Higher temperatures (e.g., 1.5) increase creativity and variability, but may risk generating less coherent or relevant content, making it important to adjust based on your desired outcome. |
| Max Tokens            | No       | Sets a limit on the number of tokens (words, characters, or pieces of text) in the model’s response. |
| Reasoning effort      | No       | Guides the model on how much to think when performing a task.  |
| API type              | No       | Specifies which API type to use for the selected AI model. Valid values are `Completions` and `Responses` . You should **not** specify this property unless you want to change the default which is `Responses`  |
| Description           | No       | Additional details or notes. |

<br/>