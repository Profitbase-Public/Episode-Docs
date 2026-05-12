# Search vectors

Performs a vector search agains a SQL Server / Azure SQL database and returns an [IVectorSearchResult](../../api-reference/built-in-types/ai/i-vector-search-result.md) object that can be passed to a Chat completion action.


![SQL Vector Search](../../../../images/flow/sql-vector-search.png)


**Example** ![Example](../../../../images/strz.jpg)  
This flow processes a user's chat question by first receiving it through a [Chat completion](../../triggers/ai/chat-completion-trigger.md) trigger, then converting it into a vector using a [text embedder](../azure-ai/text-embedder.md), performing a **Vector search** in an SQL database for relevant context, and finally passing the user input and retrieved context to an Azure AI [Chat completion](../azure-ai/chat-completion.md) action, which generates a response that is returned to the client via the [Return](../built-in/return.md) node.

</br>

# Properties

| Name                         | Required | Description                                                                                      | 
|------------------------------|-----------|--------------------------------------------------------------------------------------------------|
| Title                        | No | The title or name of the action. |
| Connection                   | Yes | The SQL Server database connection that will be used for the vector search. |
| Table                        | Yes | The name of the table where the vector search will be performed. |
| Search text                  | Yes | The input text string used to perform the vector search; can be provided dynamically. |
| Record definition            | Yes | The definition of columns used in the vector search. Note: The vector column is not returned. See example below. |
| Filter                       | No | A filter expression to narrow down the records (e.g., category = blue). |
| Top                          | No | The maximum number of top results to return. |
| Skip                         | No | The number of top results to skip (default: *0*). |
| [Distance function](https://learn.microsoft.com/en-us/azure/cosmos-db/gen-ai/distance-functions) | No |  The method for calculating vector similarity, e.g., *Cosine Distance* (default). |
| Score limit | No |  A threshold value that limits results to those with a distance or similarity score at or below this value. The meaningful range depends on the selected **Distance function**. Tune by observing actual scores returned for your data. |
| Prompt template              | No | The prompt template controls the output format for your search results. The system replaces placeholders, such as @[fieldName], with the corresponding data from your vector search fields. See example below. |
| Search result variable name  | No | The name of the variable to store results, e.g. searchResult. |
| Command timeout (seconds)    | No | The timeout duration for the SQL command (in seconds). |
| Description                  | No | Additional notes or metadata for the action. |

<br/>

## Returns 

The Search vectors action returns an [IVectorSearchResult](../../api-reference/built-in-types/ai/i-vector-search-result.md) object that can be passed to the Chat completion Context property.

<br/>

#### Record definition

You are required to specify the *key*, *content* and *vector* columns as **Field definitions**. ![Example](../../../../images/strz2.jpg) 

![SQL Vector Search1](../../../../images/flow/sql-vector-search1.png)

<br/>

#### Prompt template

The prompt template allows you to customize the format of the output from your vector search. This is useful for structuring the search results in a way that is most useful for your application.
You can use placeholders in the format @[fieldName] to include specific data from your search results. The system will replace these placeholders with the actual values from the corresponding fields in the search results.

**Example** ![Example](../../../../images/strz2.jpg) 

![SQL Vector Search Prompt Template](../../../../images/flow/sql-vector-search-prompt-template.png)  

<br/>

[!INCLUDE [](__videos.md)]