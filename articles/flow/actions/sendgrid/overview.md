# SendGrid overview

Flow includes built-in support for sending transactional and notification emails through [SendGrid](https://sendgrid.com/). Common use cases include sending automated confirmations after a form submission, delivering generated reports as email attachments (such as a PDF built earlier in the flow), or notifying recipients when an event triggers a flow.

To use the action in this category, you first need a [SendGrid connection](./sendgrid-connection.md) configured with a SendGrid API key and one or more approved sender addresses. The same connection can hold multiple senders — useful when a single integration sends emails on behalf of different departments or services.

<br/>

## Explore

#### Connection
Set up the SendGrid connection used by the Send email action. Requires an API key with `Mail Send` permission and at least one approved sender (email address with optional display name). If domain authentication is enabled in SendGrid, sender addresses must be verified — either through domain authentication or individual sender verification.  
[Read more](./sendgrid-connection.md)

<br/>

#### Sending email
[Send email](./send-email.md) sends an email through the configured connection. Specify the **From email** (must match one of the verified senders in the connection), a **From name** for the display, one or more recipients in the **To** field separated by semicolons, a subject, and a message body — plain text or basic HTML. Optional attachments can be included as part of the email.
