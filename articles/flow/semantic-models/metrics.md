# Metrics (Derived)

Metrics in a semantic model enables combining multiple simple metrics from (different) datasets into a single derived metric defined by an expression, for example `sales - cost_of_goods_sold` where `sales` and `cost_of_goods_sold` are simple metrics in one or more datasets in the semantic model.  

Before you can define any derived metrics, you must add at leas one [Dataset with a simple metric](./datasets.md#dataset-metrics).

> [!IMPORTANT]
> To use metrics from different datasets in the same expression, it is required that they share at least one common column relationship. 

## Properties
| Name           | Description                         |
|----------------|-------------------------------------|
| Name           | The name of the metric as presented to the AI agent. Use a descriptive name to help the agent understand how to use the metric.|
| Description    | Optional, but recommended. A short, informative description that helps the AI agent understand what the metric represents and how to use it. |
| Expression     | The formula of the metric. You can use numbers and simple metrics defined in datasets as operands. Allowed operators are +,-,*, / and parentheses for grouping. <br/><br/> Example: `(sales - cogs) / sales * 100`, where `sales` and `cogs` are metrics defined in one or more datasets. |
| Synonyms       | Optional. A list of alternative terms for the metric `Name`. Add synonyms when several terms are commonly used for the same metric, so the AI agent can pick the correct metric from a user request. |

