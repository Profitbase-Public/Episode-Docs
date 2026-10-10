# Datasets

A dataset is a projection of a database table or view. It describes the source's schema (columns, data types, relationships) and its metrics, so that an AI agent can query the data through the [MCP endpoint of the semantic model](./connect-mcp-clients.md).

Think of a dataset as a table wrapped in structured information that tells the AI agent how to query the table and use it for data analysis.

## Properties

| Name        | Description |
|-------------|-------------|
| Source      | The name of the database table or view that the dataset references. |
| Description | Describes what the dataset contains. Optional, but recommended: it helps the AI agent understand what the data contains and when to use it. |

<br/>

## Columns

Columns define which columns of the source table or view are exposed to the AI agent.

| Property    | Description |
|-------------|-------------|
| Column name | The name of the column in the source table or view. The AI agent uses it to build data queries. |
| Data type   | The logical data type of the column. It tells the AI agent which operations it can apply. For example, if a column is numeric, the agent knows it can aggregate it or use it in math expressions. |
| Description | Recommended. Describes what the column contains, for example `Total number of cars sold in the current period`. |

#### Column details

The column details define the `role` of a column in a dataset, such as it's relationship to other columns in the semantic model (typically Primary key / Foreign key), or its dimensionality role (categorical, time filter or time aggregation / rollup).



<br/>

## Dataset metrics

