# GraphQL request overview

Flow includes built-in support for calling [GraphQL](https://graphql.org/learn/)-compatible APIs through the [GraphQL request](./graphql-request.md) action. The action executes both queries (for reading data) and mutations (for writing or modifying data), supports GET and POST methods, and works with standard GraphQL features such as filters, arguments, fragments, aliases, and operation names.

The request is configured through a single Configuration property, which lets you set the endpoint URI, HTTP method, the query or mutation itself, variables, custom HTTP headers (for example for authentication), and the expected response type. The result is returned as the object type defined in the configuration — typically used as input to actions that flatten or convert the response, such as [Get JSON DataReader](../json/get-json-datareader.md), before being inserted into a database or processed further.
