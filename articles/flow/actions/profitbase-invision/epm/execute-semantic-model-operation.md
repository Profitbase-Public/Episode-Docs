# Execute Profitbase B&F Semantic Model operation

Makes data and metadata from a **Profitbase Budgeting and Forecasting (B&F)** solution available to AI agents through the [Flow Semantic Model MCP server](../../../semantic-models/connect-mcp-clients.md).  

The action updates a [semantic model](../../../semantic-models/overview.md) using the specified [Operation](#operations).  

<br/>

![Execute Profitbase B&F Semantic Model operation](/images/flow/invision-epm-operation.png)  
**Example** ![Example](/images/strz.jpg)  
Publishes financial plan data and actuals to the semantic model data source, and updates or creates the corresponding semantic models.

<br/>

## Properties

| Name          | Required         | Description            |
|---------------|------------------|------------------------|
| Export name   | Yes              | Identifies the data set in the Profitbase B&F solution to make available to the semantic model and AI agents. |
| Business domain friendly name | No | An alias for the export name, used to give semantic model resources business-oriented names when the export name is technical. For example, an `Export name` in Planner may be a technical code like `AI_Export_Forecast`, which conveys little business meaning to an AI model. A friendly name like `Forecast` makes the context of the data set easier for both people and AI agents to understand. |
| Operation     | Yes              | Specifies which operation to apply to the semantic model. [See details below](#operations). |

<br/>

## Operations

| Operation                      |  Description                                      |
|--------------------------------|---------------------------------------------------|
| `Publish plan data for the semantic model` | Publishes financial plan data (budget or forecast) defined by the specified `Export name` in Profitbase to a table in the InVision database. The table is named `[epm].[<export name>_financial_metrics]`, or `[epm].[<business domain friendly name>_financial_metrics]` if a business domain friendly name is specified. |
| `Build the semantic model for plan data` | Builds (creates or updates) the semantic model for financial plan data (budget or forecast) based on the current `Report Lines` definition. This determines which metrics, dimensions and datasets are available to AI agents. Requires that the data source for the semantic model already exists, meaning `Publish plan data for the semantic model` must have run first. |
| `Publish Actuals for the semantic model`| Publishes actuals in Profitbase to a table in the InVision database. The table is named `[epm].[actual_financial_metrics]`, or `[epm].[<business domain friendly name>_financial_metrics]` if a business domain friendly name is specified. |
| `Build the semantic model for Actuals` | Builds (creates or updates) the semantic model for actuals based on the current `Report Lines` definition. This determines which metrics, dimensions and datasets are available to AI agents. Requires that the actuals data source for the semantic model already exists, meaning `Publish Actuals for the semantic model` must have run first. |

<br/>

## Where and when to use this action

**Where**  
Run this action in a Flow in the **same Workspace as the semantic model you want to update**. This is typically the `EPM Reporting` Workspace connected to the `EPM Reporting` InVision Solution that contains the master `Report Lines` definition.  

**Order of execution**  
A semantic model can only be built or updated when the underlying data structures are in place. Run the operations in this order:
- Operation [`Publish plan data for the semantic model`] before Operation[`Build the semantic model for plan data`].
- Operation[`Publish Actuals for the semantic model`] before Operation[`Build the semantic model for Actuals`].

Each pair can run in the same Flow or in different Flows.

**When**  
- After financial plan data has been updated (typically after a driver or financial calculation completes), run `Publish plan data for the semantic model` to refresh the data.
- After actuals have been updated (typically after a data import from the ERP system or data warehouse/platform), run `Publish Actuals for the semantic model` to refresh the data.
- When the `Report Lines` definition has changed, run `Build the semantic model for plan data` and `Build the semantic model for Actuals` to create the semantic model or update its configuration. This controls which metrics, dimensions and datasets are available to AI agents.

<br/>

## Data published to the semantic model

The following data from Profitbase is published to the semantic model data source:
- All dimensions in use, except `Currency`. Currency is excluded because the published data set only contains amounts in the `Reporting currency`.
- All `Report Lines` marked as `AI Enabled`. For `AI Enabled` computed report lines (those with formulas), all report lines referenced in the formulas are exported automatically.
- Data from Work Process Versions tagged with the specified `Export name`.
- Actuals stored in Profitbase.
- All amounts are in `Reporting currency`.
