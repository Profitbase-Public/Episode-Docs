# Microsoft Teams overview

Flow includes built-in support for sending messages to Microsoft Teams — either to an individual user as a 1:1 chat, or to a channel in a team. Common use cases include posting flow status updates, alerts, or daily reports to a team channel, and notifying a specific user when something requires their attention.

To use any Teams action, you first need a [Microsoft Teams connection](./connection.md). Unlike most Microsoft connections in Flow, Teams uses interactive sign-in with a **Microsoft Work or School account** rather than a Service Principal — this means messages are sent as the user who signed in, and will appear with that account as the sender.

<br/>

## Explore

#### Connection
Set up the connection used by both Teams actions. Sign-in opens in a browser window using your Microsoft Work or School account, and the resulting connection determines who messages will be sent as.  
[Read more](./connection.md)

<br/>

#### Sending messages
Two actions cover the supported targets. [Send chat message](./send-chat-message.md) sends a 1:1 message to a specific user, identified by their Microsoft Entra ID Object ID (OID) or User Principal Name (UPN). [Send message in channel](./send-channel-message.md) posts a message to a channel, identified by Team ID and Channel ID. Both support either plain text or HTML message content, both can include attachments, and both return a [chat message](./api-reference/chat-message.md) object containing the sent message's ID and body.

> [!NOTE]
> In Teams terminology, an *attachment* is a **reference** to an existing document in OneDrive or SharePoint — you cannot attach a raw file (byte array or stream) directly. To include file content directly, use **Hosted content** instead.
