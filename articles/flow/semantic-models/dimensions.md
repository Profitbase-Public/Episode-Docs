# Dimensions

Dimensions are central to a semantic model for three reasons:

- **Aggregation.** Dimensions let you roll up data over structures such as organization, product and time hierarchies. Data is usually stored at a finer granularity than the level the business reports on.
- **Relationships.** Dimensions form the backbone of the relational model that ties the datasets and business metrics together.
- **Metadata.** Dimensions describe the datasets and business metrics with attributes such as product category, color, size, weight, market, region and customer group. This enables flexible analysis across different dimensionality.

## Properties

| Name        | Description |
|-------------|-------------|
| Source      | The name of the dimension table or view that the dimension references. |
| Description | What the dimension represents. Optional, but recommended: it helps the AI agent understand the data and when to use it. |

<br/>

## Columns

Columns define which columns of the source table or view are exposed to the AI agent. This lets the agent fetch the dimension in tabular format for analysis.

| Property       | Description |
|----------------|-------------|
| Key column     | The name of a dimension column to make available to the AI agent. This includes ID columns, attribute columns and other descriptive columns. |
| Display column | The column that holds the common name for a key column. Only applicable to level ID columns such as DepartmentID and L1_ID. <br/><br/>Business systems usually store only member IDs in transactions (such as department or product IDs), not names like "Engineering" or "T-Shirts". Users, however, ask for data by name, for example: "What is the total number of FTEs in Engineering?" By pairing a key column with a display column, the AI agent can translate the name into the matching key values and filter the result correctly. |
| Column type    |  |

### Column details

Column details define the role a column plays in the semantic model: its relationships to columns in other datasets and dimensions (typically primary key / foreign key), and its place in a hierarchy.

#### Relationship

| Name              | Description |
|-------------------|-------------|
| Is relationship   | Whether the column defines a relationship to a column in another dataset or dimension. Typically enabled for key columns such as DepartmentID, AccountID and ProductID. |
| Relationship name | The name of the relationship the column belongs to. You can choose any name. All columns that share a relationship name belong to the same logical relationship, so you define a relationship by choosing a name and reusing it across datasets and dimensions. |
| Role              | `Primary`, `Foreign` or `Unique`. The relational role of the column.<br/>- Only one column in a relationship can have the `Primary` role.<br/>- Dimensions usually hold the `Primary` columns.<br/>- Dimension columns in datasets (DepartmentID, AccountID, etc.) usually have the `Foreign` role. |

> [!NOTE]
> In a dimension, enable `Is relationship` only for the leaf-level ID column (such as DepartmentID), and always give it the `Primary` role.

#### Hierarchy metadata

| Name                | Description |
|---------------------|-------------|
| Is hierarchy column | Whether the column represents a level in a hierarchy. Usually enabled only for columns that hold the member ID at each level, such as DepartmentID (leaf level), L1_ID and L2_ID. |
| Level name          | A name that tells the AI agent what the level represents, for example "Region" or "Country". If a user asks for `sales by region`, the level name lets the agent roll up the data to the correct level. |
| Level number        | The position of the level in the hierarchy. Helps the AI agent understand the structure of the dimension. |
| Parent              | The parent of the level. Helps the AI agent understand the structure of the dimension. |
