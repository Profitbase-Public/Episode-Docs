# Triggers overview

A **trigger** defines the entry point of a Flow — *how* and *when* the Flow starts running, and *what data* it receives at startup. Every Flow has exactly one trigger, which determines whether it runs on a schedule, in response to an HTTP request, when a new message lands in a queue, when a user saves data in Power BI, when an AI agent decides to call it, or any of the other patterns supported below.

Triggers are added the same way as actions — from the trigger library in the Flow Designer. Each trigger exposes its received data as one or more variables that the rest of the Flow can read and process.

<br/>

## Explore

#### Schedule and manual triggers
Triggers that don't depend on an external event. [Schedule trigger](./schedule-trigger.md) runs the Flow at a configured frequency — useful for periodic jobs such as nightly data syncs or hourly cleanups. [Flow trigger](./flow-trigger.md) defines the entry point for a Flow that is called from another Flow, enabling reusable sub-flows and nested execution. [Multi-trigger](./multi-trigger.md) lets a single Flow respond to multiple sources at once — typically a combination of HTTP, Flow, and Schedule — with a `TriggerName` field on the output telling the Flow which source actually triggered the current run.

<br/>

#### HTTP requests
[HTTP trigger](./http-trigger.md) starts a Flow when an HTTP request is received — turning the Flow into an HTTP-callable endpoint that third-party applications, custom apps, or InVision Workbooks can invoke. The trigger defines how the request body is converted into a typed object, and the Flow can return data (or a file, via [Return File HTTP response](./../actions/http/return-file-http-response.md)) as the HTTP response. Note that an [API Key](./../tenants/api-keys.md) with `Execute` permissions must be defined on the tenant before the Flow has a public endpoint.

<br/>

#### Email
Two triggers fire when a new email arrives in a Microsoft 365 mailbox. [When a new email arrives](./microsoft-365-outlook/when-new-email-arrives-trigger.md) monitors a personal mailbox, and [When a new email arrives in a shared mailbox](./microsoft-365-outlook/when-new-email-arrives-in-shared-mailbox-trigger.md) monitors a shared one. Both require a [Microsoft 365 Outlook connection](./../actions/microsoft-365-outlook/outlook-connection.md) created by signing in with an account that has access to the target mailbox. Common use cases include processing invoice attachments, monitoring a support inbox, or routing incoming messages to downstream systems.

<br/>

#### Messaging and event streams
Four triggers integrate with messaging systems. [Azure Service Bus Queue trigger](./azure-service-bus/queue-trigger.md) and [Azure Service Bus Topic trigger](./azure-service-bus/topic-trigger.md) start the Flow when a new message is received from a Service Bus queue or topic subscription. [Azure Event Hub trigger](./azure-event-hub/event-hub-trigger.md) fires when a message arrives in an Event Hub. [RabbitMQ message trigger](./rabbitmq/message-trigger.md) starts the Flow when a message arrives on a RabbitMQ queue (including topic subscription queues). Use these for event-driven processing — order events, telemetry, system notifications, anything published from another system.

<br/>

#### File storage events
Two triggers monitor Azure Blob Storage. [Blob trigger](./azure-blob-storage/blob-trigger.md) periodically lists all blobs in a container and starts the Flow with the full list — useful when you need to act on the current state of the container regardless of when files arrived. [Incremental Blob trigger](./azure-blob-storage/incremental-blob-trigger.md) only picks up new or modified blobs since the last check — preferred when each blob should be processed exactly once, such as for incoming files in an ingestion pipeline.

<br/>

#### Power BI writeback
[Writeback Table trigger](./power-bi/writeback-table-trigger.md) starts the Flow when a user presses **Save** in the [Profitbase Writeback Table](./../../PowerBI/writeback-table/overview.md) Power BI visual, delivering a [DeltaSet](./../api-reference/built-in-types/deltaset.md) of the row-level inserts, updates, and deletes the user made. [Writeback Comments trigger](./power-bi/writeback-comments-trigger.md) does the same for the [Writeback Comments](./../../PowerBI/writeback-comments/overview.md) visual. The DeltaSet is then typically passed to a [Save DeltaSet](./../actions/sql-server/save-deltaset.md) action targeting the destination database (SQL Server, Snowflake, or another supported store).

<br/>

#### Hypergene InVision
Four triggers integrate Flow with InVision. [User chat trigger](./invision/user-chat-trigger.md) handles notifications from the [InVision User Chat component](./../../invision/docs/workbooks/components/userchat/user-chat.md). [Send tabular data trigger](./invision/send-tabular-data-trigger.md) handles **Send Data** requests from InVision tabular components such as Worksheets, Tables, SQL Reports, and Table Views — useful for processing tabular data displayed in a Workbook without going through the InVision database or API. [Chat completion trigger](./ai/chat-completion-trigger.md) is the entry point for Flows that serve as the backend for an interactive AI chat, with built-in support for passing context (for RAG patterns) alongside the user's prompt. [Chat feedback trigger](./ai/chat-feedback-trigger.md) handles thumbs-up / thumbs-down feedback for responses generated by the [InVision AI Chat component](./../../invision/docs/ai-chat/overview.md).

<br/>

#### Exposing Flows to AI agents
Two triggers turn a Flow into a callable tool for AI systems. [Flow AI tool trigger](./ai/flow-ai-tool-trigger.md) makes the Flow callable by an AI agent (such as an OpenAI-based agent inside another Flow), receiving arguments and returning structured output back to the agent. [MCP tool trigger](./mcp/mcp-tool-trigger.md) exposes the Flow over the [Model Context Protocol](https://modelcontextprotocol.io), making it discoverable and callable by any MCP-compatible LLM or agent. Use these to turn business logic, data lookups, or integrations into autonomous tools that AI agents can invoke during their reasoning.
