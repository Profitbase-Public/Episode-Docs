# Flow May 2026 update

The May 2026 update of Hypergene Flow brings a broad set of improvements across AI, data integration, and developer tooling. Highlights include grounding, token usage reporting, and built-in chat memory for AI chat completions, a new Vector Search tool and Load Delta Table action for SQL Server, and a new RabbitMQ integration with both publish and trigger support. The release also adds a Hogia ERP connector, new PDF conversion options (including PDF-to-image and multiple Markdown engines), expanded Prophet forecasting capabilities, additional Outlook email properties, and a range of smaller enhancements to Snowflake, GitHub, Visma.Net, Tavily, packaging, and the Portal UI.

<br/>

## AI Chat completions

**Grounding**  
All chat completion actions now support grounding.  
If you enable this feature, chat completions will search the internet for relevant information in addition to relying on training data and context (such as data from a RAG system).  

![img](/images/flow/azure-ai-chat-completion-enable-grounding.png)

**Token usage**  
All chat completion actions (e.g., Microsoft Foundry, OpenAI, Anthropic, Google Vertex AI) now report token usage, so you can monitor and limit consumption accordingly.  

**Chat memory**  
All new chat completion actions now have built-in support for agent memory through the `Memory` property port. It lets chat agents manage their own memory using a memory provider, such as the AI agent memory tool for SQL Server, instead of requiring every developer to implement memory management manually. This enables agents to hold long, continuous conversations with users rather than starting over with each new question.

![img](/images/flow/azure-ai-chat-memory.png)

<br/>

## Tools AI agent - AIContent input

The [Tools AI agent](../actions/agents/tools-ai-agent.md) now accepts files and DataTables as direct input, allowing agents to reason about data sets and file contents such as images (OCR) and PDFs. Note that not all LLMs support image or PDF files, so you need to refer to the LLM documentation to know which formats are supported.

![img](/images/flow/tools-ai-agent-ai-content.png)

<br/>

## AI Chat - feedback trigger
The AI Chat feedback trigger enables receiving feedback ("thumbs up or down") from AI chats through Flow and storing the information in a database. This is useful for improving the quality of AI-generated answers based on user feedback.  

<br/>

## Developer tooling
When you run Flows interactively from the Designer, Flow automatically collects data at runtime so you can inspect it in the debugger screens. The data collection uses server resources and is generally not a concern. However, if you have a combination of low-resource servers and complex flows moving large amounts of data, you may experience out-of-memory errors in Dev Mode. Starting with Flow 1.13 (May 2026), you can now disable debugging on a per-Flow basis when running in Dev Mode from the Designer.  

![img](/images/flow/run-without-debugging.png)

<br/>

## GitHub  
We've added support for the `Get Issue` action which enables fetching a `specific` issue from GitHub rather than all.  

<br/>

## Hogia ERP connector
Flow now has a built-in connector for [Hogia](https://www.hogia.se/), which enables building data integrations to the Hogia ERP system. 

![img](/images/flow/hogia-connectors.png)

<br/>

## Outlook
**SentDateTimeUTC property**  
Email messages fetched from Outlook now include the SentDateTimeUTC property. It makes it easier to create integrations to Outlook where time stamps matter, for example progressively fetching new messages to build a knowledge base for ticketing / support systems.  

**UniqueBody**  
Email messages fetched from Outlook now include the `UniqueBody` property.  
When replying to an email in Outlook, the entire message (including the previous messages, reply, subject, from, to etc) is included in the default body property. UniqueBody contains only the reply, not the entire thread, making it easier to extract information from email threads without duplication of data or noise such as subject, sender and receiver. 

**InternetMessageHeaders (advanced)**  
Email messages fetched from Outlook can now optionally include the `InternetMessageHeaders` property.  
This property contains low-level metadata and routing information for the email message and is intended for advanced use only. Among other things, it provides the data needed to recreate a conversation thread in the correct order, even if messages have been moved between folders during the conversation.  

<br/>

## Packaging
You can now specify to include secrets (like passwords) in Workspace Variables when exported in Flow Packages. This option is off by default but can be enabled pr Workspace Variable.

<br/>

## PDF

**PDF to Markdown - multiple conversion engines**  
The [Convert PDF to Markdown action](../actions/pdf/convert-to-markdown.md) now supports selecting different conversion engines: `MarkItDown`, `Docling`, and `Kreuzberg`. Each engine has its own strengths and weaknesses, so you can choose the right one for your use case. Docling typically excels at complex or scanned PDFs but is slower and more resource-intensive. MarkItDown and Kreuzberg are faster but may be less accurate for complex PDFs.  

**Convert PDF to image**  
We now support [converting PDFs to images](../actions/pdf/convert-to-image.md), which enables AI agents to more easily reason about the contents of files, such as extracting invoice information. This action is an alternative to using the Adobe PDF actions for PDF conversion.  

![img](/images/flow/pdf-convert-to-image.png)

<br/>

## UI enhancements (Portal)
- Switched to Hypergene theme for both the Portal and Designer.
- If you open the same flow from the portal multiple times, the browser will not simply switch to the existing tab instead of opening a new tab every time.
- Bug fix: Double clicking a folder in the Workspace section now navigates into the Workspace and folder instead of just to the Workspace

<br/>

## Prophet (Predictive forecasting)
**Holidays**  
You can now specify for which countries public holidays should be considered when making predictions.  

**Grouping**  
This feature enables making predictions for multiple data sets at the same time, for example multiple departments or product groups. If you don’t specify a grouping, the data source must contain historical data for only a single dataset in the y and ds columns.  

**Data source types**  
In addition to Parquet and CSV files (byte array), the Prophet prediction action now supports DataReader, DataTable and Stream as input (Stream is a CSV or Parquet file stream). This basically simplifies the use of the action, as you don't need a separate action to convert data before passing it to the predict action.  

**Time zones**  
The prediction engine now supports dates (ds) containing time zones. These values are converted to UTC dates internally, and the output (predicted data set) always contains dates in UTC format.

**Linear and logistic growth**  
We added support for both linear and logistic growth.  
Logistic growth is used for data that saturates at a specific maximum capacity (defined by a cap or floor value), such as market adoption or population growth, while linear is used to predict data with no limit like website traffic.

<br/>

## RabbitMQ
You can now create integrations to RabbitMQ using Hypergene Flow, meaning you can publish messages to RabbitMQ queues, and listen for messages to automatically run Flows as they arrive.

**Publish message**  
The [Publish message action](../actions/rabbitmq/publish-message.md) enables notifying and passing data to other systems through RabbitMQ.  

**RabbitMQ trigger**  
The [RabbitMQ trigger](../triggers/rabbitmq/message-trigger.md) enables Flow to listen for messages arriving in a RabbitMQ queue and automatically run as they arrive. This means you can use Flow to automate business workflows using RabbitMQ as a message bus. 

![img](/images/flow/rabbitmq-trigger-release-notes.png)

<br/>

## Snowflake
The [Save DeltaSet action](../actions/snowflake/save-deltaset.md) for Snowflake got an update so it’s as easy to use as its SQL Server counterpart. When data is saved from a PowerBI writeback visual, this action will now automatically perform either an insert or update based on the data in the payload with no or very little configuration required.

<br/>

## SQL Server / Azure SQL

**Incremental data load for data integrations**  
The new [Load Delta Table action](../actions/sql-server/load-deltatable.md) greatly simplifies building data integration pipelines with support for incremental loads, applying only the changes (new, updated, and deleted rows) to the target system since the last update.  

![img](/images/flow/sql-server-load-deltatable-release-notes.png)

**Developer productivity**  
We improved the search feature in the SQL object explorer. Search results are now displayed in a hierarchical view — if you search for a column, for example, its parent table appears in the results pane as the ancestor of the matched column name.  

**Vector search AI tool**  
The new [Vector Search tool](../actions/sql-server/vector-search-tool.md) makes it easier to build AI agents that use vector databases to fetch relevant information. The tool lets the agent determine when additional context is needed and perform the appropriate search to reply to questions. For example, if the agent determines a question is a follow-up question to a previous answer, it can determine whether it needs to fetch additional information, or simply look at the chat history. 

![img](/images/flow/sql-server-vector-search-ai-tool.png)

<br/>

## Tavily
Tavily provides web search for AI agents, and the May 2026 update of Flow adds support for additional search parameters including 
- Search depth (how deep Tavily follow links in a page)
- Topic filter
- Day limit (basically how up-to-date information to search for) 
- Domain exclusion (domains to exclude from search)  

<br/>

## Visma.Net
In addition to the current [Paged API request action](../actions/visma/visma-net/paged-rest-api-request.md), the new [API request action](../actions/visma/visma-net/rest-api-request.md) supports standard, non-paged API requests, typically used to send data to Visma.Net or fetch small amounts of data such as single entities. 

<br/>  

## Miscellaneous
-	The [Get JSON DataReader action](../actions/json/get-json-datareader.md) and [Convert JSON to DataTable action](../actions/json/get-json-datatable.md) now supports the new `Root path` property which enables reading data from a specific property within a JSON document instead of the root of the document itself.
-	Improved resilience for Entra ID connectors
