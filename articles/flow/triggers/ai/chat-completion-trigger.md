# Chat completion trigger

Defines the entry point for a Flow that executes an AI chat completion request.  

You'll typically use this trigger when you want to implement a Flow that serves as the backend for an interactive chat, allowing users to have a conversation with an AI by asking questions and follow-up questions. The trigger also provides a context property, allowing arbitrary data to be passed to the Flow to use for filtering or other types of business logic, supporting workloads such as RAG to help the LLM generate more relevant responses.

![Chat Completion Trigger](/images/flow/chat-completion-trigger.png)

**Example**![Example](../../../../images/strz.jpg)  
The example above shows a Flow that performs a chat completion. It supports follow-up questions by loading the chat history for the session, feeding it into the chat model, and then saving the updated chat history (including the response) back to the database before returning the response to the client.
<br/>

## Properties

| Name        | Required | Description                     |
|-------------|---------------|---------------------------------|
| Title       | No            | A descriptive label for the trigger.    |
| Test data   | No            | A JSON string that can be in dev mode to test the Flow before it's called from the outside (for example InVision). The JSON must have format as described in [Test data](#test-data) below. |
| Output variable name | Yes       | The name of the variable containing the `ChatCompletionRequest` object. (See [Returns](#returns below))  |

### Test data  
The Test data property must have the following format:  
```json
{
    "sessionId": "some-id",
    "prompt": "some question",
    "context": "some context"
}
```
<br/>

## Returns
The trigger outputs a `ChatCompletionRequest` object with the following properties.  

| Property name  | Type    | Description              |
|----------------|---------|--------------------------|
| SessionId      | String  | The id of the chat session provided by the client. Use this value to store and retrieve the chat history between requests, enabling users to ask follow-up questions. |
| Prompt         | String  | The question asked by the user. |
| Context        | String  | A value provided by the client (for example a product id, file name or JSON string) that can be used to provide context to the chat completion process (for example as additional parameters to vector search) |

<br/>

## Description
To perform a chat completion, you need at least a `user prompt` (the question asked by the user). This is provided by the `Prompt` property of the `ChatCompletionRequest` object returned from the trigger (described above), and can be mapped to the `User prompt` property of an AI chat completion action, such as the [Azure AI chat completion](../../actions/azure-ai/chat-completion.md) action. You should also enable the user to have a conversation (asking follow-up questions) with the chat model by managing the chat history. For this purpose, use the `SessionId` to store and retrieve the chat history between requests.

Note that a Flow (and consequently a chat completion) is stateless, so to enable users to have a conversation with an AI by asking follow-up questions, you need to provide the chat history to the chat model along with the user prompt and any other data for _every_ request. For this purpose, you can use the `SessionId` provided by the `ChatCompletionRequest` object to store and retrieve the chat history from a database between requests. The SessionId is provided by the chat client, allowing users to start new sessions when they want.
