# Triggers overview

A **trigger** defines the entry point of a Flow — *how* and *when* the Flow starts running, and *what data* it receives at startup. A Flow can have at most one trigger, which determines whether it runs on a schedule, in response to an HTTP request, when a new message lands in a queue, when a user saves data in Power BI, when an AI agent decides to call it, or any of the other patterns supported below. A Flow without a trigger can still be executed manually from the Designer or [called from another Flow](../actions/built-in/run-flow.md).

Triggers are added the same way as actions — from the trigger library in the Flow Designer. Each trigger exposes its received data as one or more variables that the rest of the Flow can read and process.

<br/>

## Explore

#### Schedule and manual triggers
[Schedule trigger](./schedule-trigger.md) runs the Flow at a configured frequency — useful for periodic jobs such as nightly data syncs or hourly cleanup jobs. [Flow trigger](./flow-trigger.md) defines the entry point for a Flow that is called from another Flow, enabling reusable sub-flows and nested execution. [Multi-trigger](./multi-trigger.md) lets a single Flow respond to multiple sources at once — typically a combination of HTTP, Flow, and Schedule — with a `TriggerName` field on the output telling the Flow which source actually triggered the current run.

<br/>

#### HTTP requests
[HTTP trigger](./http-trigger.md) starts a Flow when an HTTP request is received — turning the Flow into an HTTP-callable endpoint that third-party applications, custom apps, or InVision Workbooks can invoke. The trigger defines how the request body is converted into a typed object, and the Flow can return data (or a file, via [Return File HTTP response](./../actions/http/return-file-http-response.md)) as the HTTP response. Note that an [API Key](./../tenants/api-keys.md) with `Execute` permissions must be defined on the tenant before the Flow has a public endpoint.

<br/>

#### Email
[When a new email arrives](./microsoft-365-outlook/when-new-email-arrives-trigger.md) starts the Flow when a new email arrives in a personal Microsoft 365 mailbox. [When a new email arrives in a shared mailbox](./microsoft-365-outlook/when-new-email-arrives-in-shared-mailbox-trigger.md) does the same for a shared mailbox. Both require a [Microsoft 365 Outlook connection](./../actions/microsoft-365-outlook/outlook-connection.md) created by signing in with an account that has access to the target mailbox. Common use cases include processing invoice attachments, monitoring a support inbox, or forwarding incoming messages to other systems.

<br/>

#### Messaging and event streams
[Azure Service Bus Queue trigger](./azure-service-bus/queue-trigger.md) and [Azure Service Bus Topic trigger](./azure-service-bus/topic-trigger.md) start the Flow when a new message is received from a Service Bus queue or topic subscription. [Azure Event Hub trigger](./azure-event-hub/event-hub-trigger.md) fires when a message arrives in an Event Hub. [RabbitMQ message trigger](./rabbitmq/message-trigger.md) starts the Flow when a message arrives on a RabbitMQ queue (including topic subscription queues). Use these for processing events published by other systems — order events, system notifications, or anything coming through a queue or stream.

<br/>

#### File storage events
[Blob trigger](./azure-blob-storage/blob-trigger.md) periodically lists all blobs in an Azure Blob container and starts the Flow with the full list — useful when you need to work with the current contents of the container regardless of when files arrived. [Incremental Blob trigger](./azure-blob-storage/incremental-blob-trigger.md) only picks up new or modified blobs since the last check — preferred when each file should be processed exactly once.

<br/>

#### Power BI writeback
[Writeback Table trigger](./power-bi/writeback-table-trigger.md) starts the Flow when a user presses **Save** in the [Profitbase Writeback Table](./../../PowerBI/writeback-table/overview.md) Power BI visual, delivering a [DeltaSet](./../api-reference/built-in-types/deltaset.md) of the inserts, updates, and deletes the user made. [Writeback Comments trigger](./power-bi/writeback-comments-trigger.md) does the same for the [Writeback Comments](./../../PowerBI/writeback-comments/overview.md) visual. The DeltaSet is then typically passed to a [Save DeltaSet](./../actions/sql-server/save-deltaset.md) action targeting the destination database (SQL Server, Snowflake, or another supported store).

<br/>

#### Hypergene InVision
[User chat trigger](./invision/user-chat-trigger.md) handles notifications from the [InVision User Chat component](./../../invision/docs/workbooks/components/userchat/user-chat.md). [Send tabular data trigger](./invision/send-tabular-data-trigger.md) handles **Send Data** requests from InVision tabular components such as Worksheets, Tables, SQL Reports, and Table Views — useful for processing tabular data displayed in a Workbook without going through the InVision database or API. [Chat completion trigger](./ai/chat-completion-trigger.md) is the entry point for Flows that serve as the backend for an interactive AI chat, with built-in support for passing context alongside the user's prompt — useful for RAG-style flows. [Chat feedback trigger](./ai/chat-feedback-trigger.md) handles thumbs-up / thumbs-down feedback for responses generated by the [InVision AI Chat component](./../../invision/docs/ai-chat/overview.md).

<br/>

#### Exposing Flows to AI agents
[Flow AI tool trigger](./ai/flow-ai-tool-trigger.md) makes a Flow callable by an AI agent (such as an OpenAI-based agent running inside another Flow), receiving arguments from the agent and returning structured output back to it. [MCP tool trigger](./mcp/mcp-tool-trigger.md) exposes a Flow over the [Model Context Protocol](https://modelcontextprotocol.io), making it available to any MCP-compatible LLM or agent. Use these to turn business logic, data lookups, or system integrations into tools that AI agents can call autonomously.
