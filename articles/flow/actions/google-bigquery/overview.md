# Google BigQuery overview

Flow includes built-in support for executing SQL queries against [Google BigQuery](https://cloud.google.com/bigquery/docs/introduction), letting you read data from a BigQuery dataset and use the results in the rest of your flow — for example, querying analytics data and inserting the rows into another database, or feeding them into downstream processing.

To use any BigQuery action, you first need a [Google BigQuery connection](./connecting-to-bigquery.md). Flow supports two authentication methods: **User Authentication** with OAuth 2.0 (Client ID, Client Secret, Refresh Token), or a **Custom JSON String** with service account credentials. Both require a Google Cloud Project with the BigQuery API enabled.

<br/>

## Explore

#### Connection
Set up the connection used by every BigQuery action. Choose between User Authentication — typically generated through tools like the OAuth 2.0 Playground with the `bigquery` scope — or pasting in the JSON key downloaded from a Google Cloud service account. The connection page covers both setups step by step, including how to enable the BigQuery API in your project.  
[Read more](./connecting-to-bigquery.md)

<br/>

#### Reading data from BigQuery
Two actions execute a SQL query and return the result, and the right one depends on how much data you have. [Get DataReader](./get-datareader.md) returns a forward-only stream of rows — typically used as input to actions that consume data sequentially, such as inserting into another database or formatting and uploading the result. [Load to DataTable](./load-to-datatable.md) loads the entire result into memory at once. Both accept a parameterized SQL expression.
