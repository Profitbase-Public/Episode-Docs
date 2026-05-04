# Search vectors

Performs a vector search against a PostgreSQL database and returns an [IVectorSearchResult](../../api-reference/built-in-types/ai/i-vector-search-result.md) object that can be passed to a Chat completion action.


![Postgres Vector Search](../../../../images/flow/postgres-vector-search.png)


**Example** ![Example](../../../../images/strz.jpg)  
This flow processes a user's chat question by first receiving it through a [Chat completion](../../triggers/ai/chat-completion-trigger.md) trigger, then converting it into a vector using a [text embedder](../azure-ai/text-embedder.md), performing a **Vector search** in a PostgreSQL database for relevant context, and finally passing the user input and retrieved context to an Azure AI [Chat completion](../azure-ai/chat-completion.md) action, which generates a response that is returned to the client via the [Return](../built-in/return.md) node.

</br>

## Properties

| Name                         | Required | Description                                                                                      |
|------------------------------|-----------|--------------------------------------------------------------------------------------------------|
| Title                    | No | The title or name of the action. |
| Connection               | Yes | The PostgreSQL database connection that will be used for the search.          |
| Table                    | Yes | The name of the table where the vector search will be performed. |
| Search text              | Yes | The input text string used to perform the vector search, can be provided by a trigger (see below). |
| Record definition       | Yes | The definition of columns used in vector search. Note! The vector column is not part of the returned data. |
| Filter                   | No | A filter expression to narrow down the records (e.g., category = blue). |
| Top                      | No | The maximum number of top results to return. |
| Skip                     | No | The number of top results to skip (default: *0*). |
| [Distance function](https://learn.microsoft.com/en-us/azure/cosmos-db/gen-ai/distance-functions)  | No | The method for calculating vector similarity, e.g., *Cosine Distance* (default). |
| Score limit    | No | A threshold value that limits results to those with a distance score at or below this score (for e.g. CosineDistance). |
| Prompt template            | No | The prompt template controls the output format for your search results. The system replaces placeholders, such as @[fieldName], with the corresponding data from your vector search fields. See example below. |
| Search result variable name | No | The name of the variable to store results. |
| Command timeout (seconds) | No | The time limit for command execution before it times out. Default is 120 seconds.  |
| Description | No | Additional notes or comments about the action or configuration.      |

</br>

#### Search text

**Example** ![Example](../../../../images/strz2.jpg) 

![Postgres Vector Search2](../../../../images/flow/postgres-vector-search2.png)

<br/>

#### Record definition

The record defintion specifies the data returned from the search.  
You are required to specify the **key**, **content** and **vector** fields in **Field definitions**. ![Example](../../../../images/strz2.jpg) 

![Postgres Vector Search3](../../../../images/flow/postgres-vector-search3.png)

<br/>

#### Prompt template

The prompt template allows you to customize the format of the output from your vector search. This is useful for structuring the search results in a way that is most useful for your application.
You can use placeholders in the format @[fieldName] to include specific data from your search results. The system will replace these placeholders with the actual values from the corresponding fields in the search results.

**Example** ![Example](../../../../images/strz2.jpg) 

![Postgresql Vector Search Prompt Template](../../../../images/flow/postgresql-vector-search-prompt-template.png)  

<br/>

## Returns 

The Search vectors action returns an [IVectorSearchResult](../../api-reference/built-in-types/ai/i-vector-search-result.md) object that can be passed to the Chat completion Context property.
