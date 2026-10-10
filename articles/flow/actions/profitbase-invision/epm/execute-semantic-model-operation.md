# Execute Profitbase B&F Semantic Model operation

Makes data or metadata from a **Profitbase Budgeting and Forecasting (B&F)** solution available to AI agents through the `Flow Semantic Model MCP server` by updating a semantic model using the specified `Operation` type.  

The `Publish Report Lines data to semantic model`-operation makes the financial metrics defined by `Report Lines` in the Profitbase B&F solution available as a dataset to the semantic model, while the `Update semantic model configuration for Report Lines`-operation creates or updates the semantic model configuration to match the current `Report Lines` definition. 

<br/>

## Properties

| Name          | Required         | Description            |
|---------------|------------------|------------------------|
| Export name   | Yes              | Identifies the data set in the Profitbase B&F solution to make available to the semantic model and AI agents. If the semantic model doesn't exist when you run `Update semantic model configuration for Report Lines`, it is created automatically with this name. |
| Operation     | Yes              | Specifies which operation to apply to the semantic model. [See details below](#operations). |

<br/>

## Operations

| Operation                      |  Description                                      |
|--------------------------------|---------------------------------------------------|
| `Publish Report Lines data to semantic model` | Copies the financial metrics defined by `Report Lines` in the Profitbase B&F solution to a table in InVision, which serves as a data source for the semantic model. The table is named `[epm].[<export name>_financial_metrics]`. This operation does **not** require an existing semantic model. |
| `Update semantic model configuration for Report Lines` | Creates the semantic model if it doesn't exist, or updates its configuration to match the current `Report Lines` definition. This determines which metrics, dimensions and datasets are available to AI agents. |

<br/>

## Where and when to use this action
**Where**  
Run this action in a Flow that is in the same Workspace as the semantic model you want to update. This is typically in the Workspace connected to the InVision Solution that contains the master `Report Lines` definition.  

**When**  
- After financial planning data has been updated (typically after a driver or financial calculation completes), run `Publish Report Lines data to semantic model` to refresh the data.
- When the `Report Lines` definition changes, run `Update semantic model configuration for Report Lines` to create the semantic model or update its configuration. This controls which metrics, dimensions and datasets are available to AI agents.

