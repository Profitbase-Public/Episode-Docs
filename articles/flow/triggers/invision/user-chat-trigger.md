# InVision User Chat notification trigger

The trigger defines the starting point of a Flow that handles [User Chat](../../../invision/docs/workbooks/components/userchat/user-chat.md) notifications from InVision. 

> [!NOTE]
> To use this trigger, a flow must be [created from, or added to InVision](../../../invision/docs/flows/how-to/add-flow-to-solution.md). In InVision, the flow must then be attached to User Chat's [Context Item](../../../invision/docs/workbooks/components/userchat/context-item.md).

<br/>

![Invision User Chat Trigger](/images/flow/invision-user-chat-trigger.png)

**Example**![Example](/images/strz.jpg)  
This flow receives a list of notifications from a [User Chat](../../../invision/docs/workbooks/components/userchat/user-chat.md) in InVision. A [For each](../../actions/built-in/foreach.md) action iterates over received notification items. An [Function](../../actions/built-in/function.md) creates a message text from the notification item, and the [Send Message](../../actions/microsoft-teams/send-chat-message.md) action forwards the message to an Microsft Teams channel.

<br/>

## Properties

| Name                 | Required | Description                                                   |
|----------------------|------------|---------------------------------------------------------------|
| Test data            | No         | Use this property to define test data to use when you run the Flow manually from the Designer during development. The test data is NOT used when data is triggered from an InVision user chat. |
| Output variable name | Yes        | The name of the variable that the output from the trigger is stored in. |

<br/>

## Returns

The trigger returns a list of `notification` that can be iterated over using a `For each` action.. 

Each `notification` (UserChatNotification) item contains the following properties:

| Name        | Description                |
|-------------|----------------------------|
| UserId      | ID of the user to receive message(s). |
| SolutionId  | The ID of the Solution in InVision. |
| ContentId   | The ID of the Chat in InVision. |
| NewMessages | List of user messages as described below (List<UserChatMessage>). |

<br/>

Each message item (UserChatMessage) in `NewMessages` property above has the following properties:

| Name         | Description               |
|--------------|----------------------------|
| Id           | The message ID.|
| UserId       | Id of the user that has added the message.|
| UserName     | Name of the User that has added the message. |
| Content      | The message text. |
| ChatName     | Name of the chat. |
| ContextText  | The Context Text is described [here](../../../invision/docs/workbooks/components/userchat/howto/context-text.md). |
| ContextLink  | The Context Link as described [here](../../../invision/docs/workbooks/components/userchat/howto/context-link.md). |
| Timestamp    | Date/time when the message was added. |
| EditTimestamp | Date/time when the message was optional edited.|

<br/>

## Test data sample

The followin `json` represents an example of an `notification` item:
```json
[
  {
    "UserId": "user-42",
    "SolutionId": "sol-456",
    "ContentId": "content-789",                
    "NewMessages": [
       {
            "Id": "msg-001",
            "UserId": "user-123",
            "UserName": "John Doe",
            "Content": "Hello. To few hours is planed for documentation. Add 15% more ?",
            "ChatName": "General Chat",
            "ContextText": "Related to project X",
            "ContextLink": "http://example.com/context-link",
            "Timestamp": "2026-06-01T10:15:30Z",
       },{                  
            "Id": "msg-002",
            "Context": "Some context",
            "UserId": "user-234",
            "UserName": "John Duo",
            "Content": "Yes, I agree, but 10% should be enough.",
            "ChatName": "General Chat",
            "ContextText": "Related to project X",
            "ContextLink": "http://example.com/context-link",
            "Timestamp": "2026-06-01T10:16:20Z",
        }
   ]
  }
]
```


