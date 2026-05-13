# Actions overview

An **action** is a building block of a Flow — a unit of work that does one thing: read a file, write to a database, call an API, run a function, send an email. A Flow is a sequence of actions wired together by their input and output ports. The Flow may also begin with a [trigger](./../triggers/overview.md) that decides *when* it runs and *what data* it receives — but that's optional; a Flow can also be run manually from the Designer or called as a sub-flow from another Flow.

Flow includes a wide library of built-in actions, organized into categories below. Most categories integrate with an external system (a database, cloud storage, an API), and require setting up a [connection](./../workspaces/workspace-objects.md) once before the actions can be used. Some categories don't need a connection at all — they operate on data already in memory.

<br/>

## Explore

#### Built-in
[Built-in](./built-in/overview.md) contains the core building blocks every Flow is built from — variables, functions, conditionals (`If`, `Case`), loops, error handling, sub-flow execution, and similar. These are used to connect steps and add logic between them.

<br/>

#### AI and machine learning
Flow integrates with the major LLM providers and supports building AI agents, generating embeddings, and using AI tools. [Agents](./agents/overview.md) provides chat agents and reusable AI components. [AI](./ai/overview.md) covers shared helpers such as text splitting and chat history management. [OpenAI](./openai/overview.md), [Anthropic AI](./anthropic/overview.md), [Azure AI](./azure-ai/overview.md), and [Google VertexAI](./google-vertexai/overview.md) provide chat completions, embeddings, and chat models for the respective providers. [Tavily](./tavily/overview.md) brings real-time web search into agents and flows. [Model Context Protocol (MCP)](./mcp/overview.md) lets Flow connect to external MCP servers and use the tools they expose. [Markdown](./markdown/overview.md) collects all conversion actions used to prepare documents for AI workflows. [Machine Learning](./machine-learning/overview.md) provides time series forecasting through the Prophet algorithm.

<br/>

#### Databases and data platforms
Relational databases and analytical data platforms. [SQL Server / Azure SQL](./sql-server/overview.md) is the most extensive — covering schema management, reading and writing data, change tracking, transactions, vector storage, and Power BI writeback. [PostgreSQL](./postgresql/overview.md) covers similar reading and writing operations and offers a strong vector store for RAG pipelines. [Snowflake](./snowflake/overview.md) targets analytics workloads, with bulk loading from staged files and Power BI writeback support. [Databricks](./databricks/overview.md) runs SQL queries against a Databricks warehouse. [Google BigQuery](./google-bigquery/overview.md) targets BigQuery datasets. [Microsoft Fabric](./microsoft-fabric/overview.md) provisions and operates Fabric workspaces, lakehouses, data pipelines, and semantic models.

<br/>

#### Files and document processing
[CSV](./csv/overview.md), [Excel](./excel/overview.md), [JSON](./json/overview.md), and [Parquet](./parquet/overview.md) cover the standard tabular and structured data formats. [PDF](./pdf/overview.md) handles PDF conversion to Markdown, image rendering, and document splitting; [Adobe](./adobe/overview.md) extends this with the Adobe PDF Services API for higher-quality conversions and structured content extraction. [Word](./word/overview.md) and [PowerPoint](./powerpoint/overview.md) convert their respective formats to Markdown. [HTML](./html/overview.md) extracts and processes HTML content, primarily for AI workflows.

<br/>

#### Cloud storage and file systems
[Amazon S3](./amazon-s3/overview.md), [Azure Blob Storage](./azure-blob-storage/overview.md), [Azure Files](./azure-files/overview.md), [Azure Table Storage](./azure-table-storage/overview.md), and [OneDrive](./onedrive/overview.md) cover the cloud-based options. [FTP](./ftp/overview.md) supports the file-transfer protocol for systems that rely on it.

<br/>

#### Messaging and streaming
For event-driven flows that publish messages to queues, topics, or event streams. [Azure Service Bus](./azure-service-bus/overview.md) supports queues and topics. [Azure Event Hub](./azure-event-hub/overview.md) targets high-volume event streams. [RabbitMQ](./rabbitmq/overview.md) publishes messages to a RabbitMQ broker. The matching [triggers](./../triggers/overview.md) consume messages from the same systems on the receiving side.

<br/>

#### Email and communication
[Microsoft 365 Outlook](./microsoft-365-outlook/overview.md) sends email from personal or shared mailboxes, reads inbox content, and processes attachments. [SendGrid](./sendgrid/overview.md) sends transactional and notification emails through SendGrid's API. [Microsoft Teams](./microsoft-teams/overview.md) sends messages to Teams users and channels.

<br/>

#### Web and HTTP APIs
For integrating with HTTP-based APIs that don't have a dedicated category. [HTTP](./http/overview.md) makes generic HTTP requests, downloads and uploads files, returns files as HTTP responses, and converts web content. [GraphQL](./graphql/overview.md) calls GraphQL endpoints. [GitHub](./github/overview.md) integrates with GitHub repositories — fetching files, listing items, working with commits. [Dynamics 365](./dynamics365/overview.md) integrates with Dynamics 365 Business Central.

<br/>

#### Identity and security
[Microsoft Entra ID](./microsoft-entra-id/overview.md) automates user lifecycle tasks (inviting guests, creating users, looking up existing ones) and monitors app registrations. [Security](./security/overview.md) provides AES encryption and decryption for content protected within a flow.

<br/>

#### Observability
[Azure Application Insights](./azure-application-insights/overview.md) sends custom telemetry — traces, events, and exceptions — to Application Insights for monitoring flow execution.

<br/>

#### Hypergene products
[Hypergene InVision](./profitbase-invision/overview.md) covers the planning, budgeting, and forecasting platform — Calculation Flows, Dimensions, Data Stores, Work Process Versions, File Storage, and custom SQL/PowerShell scripts. [Hypergene Portfolios](./hypergene-portfolios/overview.md) integrates with Hypergene Portfolios.

<br/>

#### ERP and accounting integrations
Connectors for Nordic accounting and ERP systems. [Fortnox](./fortnox/overview.md), [Hogia](./hogia/overview.md), and [Finago Office](./finago-office/overview.md) target Swedish platforms. [PowerOffice Go](./poweroffice-go/overview.md), [Tripletex](./tripletex/overview.md), [Visma](./visma/overview.md) (Visma Business NXT and Visma.Net), and [Xledger](./xledger/overview.md) target Norwegian and other Nordic platforms. Most expose REST APIs through generic *REST API request* and *REST API request with paging* actions; Visma Business NXT and Xledger use GraphQL instead. [SIE](./sie/overview.md) parses SIE files — the open Swedish standard for exchanging accounting data between systems.
