# Microsoft 365 Outlook overview

Flow makes it easy to create integrations with Microsoft 365 Outlook — sending emails (with attachments), reading inbox content, processing attachments, and exposing Outlook to AI agents as a tool. Common use cases include sending generated reports as email attachments (such as an Excel file produced from a SQL query), monitoring a shared support mailbox and saving incoming emails to a database, and processing attachments dropped into an inbox for downstream automation.

To use any Outlook action, you first need a [Microsoft 365 Outlook connection](./outlook-connection.md). Sign-in is interactive with a Microsoft 365 account, and during setup you choose the **scope** of the connection — either all mailboxes the account has access to (including the personal mailbox), or shared mailboxes only.

> [!CAUTION]
> A personal-mailbox connection grants every member of the workspace the same access — they can read, send, delete, and update messages on the connected account's behalf. For shared workflows, prefer **shared mailboxes** or sign in with a dedicated service account.

<br/>

## Explore

#### Connection
Set up the connection used by every Outlook action. Sign-in opens in a browser using a Microsoft 365 account, and you decide whether the connection covers the personal mailbox or only shared mailboxes.  
[Read more](./outlook-connection.md)

<br/>

#### Sending email
Two actions cover the sending scenarios. [Send email](./send-email.md) sends from the personal mailbox of the account used to create the connection. [Send email from a shared mailbox](./send-email-from-shared-mailbox.md) sends from a shared mailbox the connection has access to (such as `support@corp.com`), specified through the **From email** property. Both support To/Cc/Bcc with multiple semicolon-separated recipients, subject, message body, and attachments.

<br/>

#### Reading emails
Two actions iterate over emails. [For each email](./for-each-email.md) processes emails from the personal mailbox, and [For each email in shared mailbox](./for-each-email-in-shared-mailbox.md) does the same for a shared mailbox specified by email address. Both support filtering, choosing a folder, and an **Include attachments** option that decides whether attachment contents are pulled in eagerly or fetched on demand later.

<br/>

#### Working with attachments
[For each attachment](./foreach-attachment.md) iterates over the attachments of a specific email by message ID. [Get attachment](./get-attachment.md) downloads the contents of a single attachment when you didn't include attachments eagerly in the email retrieval step. Both accept an optional **Email account** parameter that's required when the email lives in a shared mailbox rather than the connection's default account.

<br/>

#### Using Outlook from an AI agent
[Outlook Agent Tool](./agent-tool.md) lets a [Tools AI Agent](../agents/tools-ai-agent.md) send emails on behalf of a user — useful when an agent needs to deliver results, summaries, or notifications via email as part of its task. A typical use is an agent that reads documents from OneDrive, summarizes them, and emails the summaries with the originals attached.

<br/>

[!INCLUDE [](./__videos.md)]
