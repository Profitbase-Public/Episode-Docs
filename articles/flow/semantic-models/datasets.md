# Datasets

A dataset is a projection of a database table or view. It describes the source's schema (columns, data types, relationships) and its metrics, so that an AI agent can query the data through the [MCP endpoint of the semantic model](./connect-mcp-clients.md).

Think of a dataset as a table wrapped in structured information that tells the AI agent how to query the table and use it for data analysis.

## Properties

| Name        | Description |
|-------------|-------------|
| Source      | The name of the database table or view that the dataset references. |
| Description | Describes what the dataset contains. Optional, but recommended: it helps the AI agent understand the data and when to use it. |

<br/>

## Columns

Columns define which columns of the source table or view are exposed to the AI agent.  
It enables AI agents to fetch the dataset in a tabular format for analysis.

| Property    | Description |
|-------------|-------------|
| Column name | The name of the column in the source table or view. The AI agent uses it to build data queries. |
| Data type   | The logical data type of the column. It tells the AI agent which operations it can apply. For example, if a column is numeric, the agent knows it can aggregate the column or use it in math expressions. |
| Description | Optional, but recommended. Describes what the column contains, for example `Total number of cars sold in the current period`. |

#### Column details

The column details define the role of a column in a semantic model. This includes its relationship to columns in other datasets and to dimensions in the semantic model (typically primary key / foreign key), and its dimensionality role (categorical, time filter or time aggregation).

**Relationship**

| Name              | Description |
|-------------------|-------------|
| Is relationship   | Specifies whether the column defines a relationship to a column in another dataset or dimension in the semantic model. Typically enabled for dimension columns such as DepartmentID, AccountID and ProductID. |
| Relationship name | The name of the relationship in the semantic model that the column belongs to. You can use any name you like. All columns in the semantic model that use the same relationship name belong to the same logical relationship. In other words, you define a relationship by choosing a name and reusing it across datasets and dimensions. |
| Role              | `Primary`, `Foreign` or `Unique`. Specifies the relational role of the column in the dataset.<br/>- Only one column within a relationship can have the `Primary` role.<br/>- Dimensions usually hold the `Primary` columns.<br/>- Dimensional columns in datasets (DepartmentID, AccountID, etc.) usually have the `Foreign` role. |

**Dimension**

| Name         | Description |
|--------------|-------------|
| Is dimension | Specifies whether the column is a dimensional column. |
| Type         | Helps the AI agent decide which columns to filter on and which columns to aggregate or roll up by.<br/>Valid values are `Categorical`, `Time filter` and `Time aggregation`.<br/><br/>- For a `Categorical` column, the agent uses the column's data type to filter it by scalar values.<br/>- For a `Time filter` column, the agent knows it can only filter by a date value or a time function (for example `last 30 days`).<br/>- For a `Time aggregation` column, the agent knows it can request a date-based rollup over the column. |

<br/>

## Dataset metrics

To add a metric to a dataset, hover over the `Metrics` folder under the dataset in the tree view, click the `...` icon to open the context menu, and click `Add` or `Add many...`.

A metric is a business calculation such as revenue, COGS, or number of active users.

> [!NOTE]
> Metrics in datasets are simple metrics: each references a single column in the dataset and applies an aggregation (SUM, AVG, MIN, MAX, etc.). To define composite metrics built from multiple simple metrics, for example a `gross profit margin %`, see [Metrics](./metrics.md).

| Name                | Description |
|---------------------|-------------|
| Name                | The name of the metric as presented to the AI agent. Use a descriptive name to help the agent understand how to use the metric. |
| Description         | Optional, but recommended. A short, informative description that helps the AI agent understand what the metric represents and how to use it. |
| Aggregation         | The aggregation the MCP server applies when the AI agent requests data for the metric. |
| Field or expression | The name of the column in the dataset to use as the metric's value source. |
| Synonyms            | Optional. A list of alternative terms for the metric `Name`. Add synonyms when several terms are commonly used for the same metric, so the AI agent can pick the correct metric from a user request. |
