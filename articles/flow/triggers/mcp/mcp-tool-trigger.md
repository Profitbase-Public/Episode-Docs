# MCP tool trigger

The `MCP tool trigger` exposes a Flow as a tool compatible with [MCP (Model Context Protocol)](https://modelcontextprotocol.io), making it accessible to LLMs.  
This means that, by adding the `MCP tool trigger` to a Flow, you enable AI agents to autonomously run Flows to complete tasks. 

![MCP Tool Trigger](/images/flow/mcp-tool-trigger.png)  

**Example** ![Example](../../../../images/strz.jpg)  
The example above illustrates a Flow that can be called by an AI agent because it has an `MCP tool trigger`. The AI agent runs the Flow as a tool through an MCP client and passes in information about the order to be placed.

<br/>

## Properties

| Name        | Required | Description                               |
|-------------|------------------|-------------------------------------------|
| Title       | No               | A human-readable title for the tool that can be displayed to users. Note that it's up to the _MCP client_ to choose whether to display the title, or just the `Tool name`.    |
| Tool name   | Yes              | The name of the tool exposed to the AI model. It is important to choose a clear, concise and descriptive name, making it easier for the AI model to reason about whether to use the tool or not. |
| Parameters  | No               | Specifies the arguments that the AI model must pass to the Flow. Make sure to use good parameter names to make it easier for the AI model to reason about which data to pass to the tool.  |
| Annotations | No               | **NOTE!** Annotations are hints about the tool. It is up to MCP clients whether to use them or not.<br/><br/>**Destructive**<br/>Specifies whether the tool may perform destructive updates to its environment. <br/><br/> **Idempotent**<br/>Specifies whether calling the tool repeatedly with the same arguments will have no additional effect on its environment.<br/><br/> **Read-only**<br/>Specifies whether this tool does not modify its environment. Read-only tools do not have side effects beyond computational resource usage. They don't create, update, or delete data in any system.<br/><br/> **Open world**<br/>Specifies whether this tool may interact with an "open world" of external entities. If true, the tool may interact with an unpredictable or dynamic set of entities, like web search.|
| Default arguments | No              | Specifies default arguments that are used when testing the Flow, or if no data is passed to the Flow from an AI model.  |
| Output variable name | Yes          | Specifies the name of the object returned from the trigger, which represents the data passed to the Flow from the AI model, for example `customerInfo` or `projectInfo`. |
| Description | No       | Defines the description of the tool to the LLM. Although this property is optional, it is strongly recommended to provide a clear and concise description of what the Flow (tool) does, so that the LLM can more easily reason about what the tool can be used for.  |

<br/>

## Returns
The MCP tool trigger returns (outputs) the object defined by its `Parameters` property (if any).

<br/>

## How to use an MCP Flow from an (external) MCP Client
Both external MCP clients (like Claude Desktop), and the [MCP Client Tool](../../actions/mcp/mcp-client-tool.md), can call MCP-enabled Flows via [MCP Server endpoints](../../flows/mcp-server-endpoints.md). You can view the MCP server endpoints from the `Resources` -> `Flow properties...` menu in the Designer, or by clicking the `Show endpoints` button in the `Property panel` of an `MCP tool trigger` node.  

<br/>

## Best practice for defining an MCP tool trigger 
1) Choose a clear and concise `Tool name`. This helps the LLM understand what the tool does and what it can be used for. Prefer using underscore (_) as word separators if the tool name consists of multiple words, for example `add_order` instead of `AddOrder`.
2) When defining `Parameters`, add a `description` for every property of the type definition.
3) When defining `Parameters`, add as few properties as possible to the type definition. Too many parameters may confuse the LLM.
4) Always provide a short and concise `Description` of the tool in the property editor. This helps the LLM reason about what the tool should be used for.  
