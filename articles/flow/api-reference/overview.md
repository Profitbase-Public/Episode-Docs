# API reference overview

This section contains reference documentation for Flow's HTTP APIs and the built-in types used across Flow definitions. Use these pages when you need exact request and response shapes for calling a Flow from an external system, looking up the structure of a built-in type returned by an action, or working with the data-manipulation classes available inside [Function](./../actions/built-in/function.md) actions.

<br/>

## Explore

#### Executing Flows
The HTTP endpoints for triggering a Flow from outside the Designer. [Run](./execute-flow/run.md) executes a Flow synchronously and returns the response, with a five-minute maximum execution time. [Run streaming](./execute-flow/stream.md) executes a Flow that streams data back to the client as it's produced — useful for chat-style responses. [Submit](./execute-flow/submit-long-running.md) starts a long-running Flow and returns a job ID immediately, and [Poll](./execute-flow/poll.md) checks the status of a submitted job and returns its log entries.

<br/>

#### Logs
The HTTP endpoints for retrieving Flow execution history. [Get Runs](./logs/get-runs.md) lists past runs of a specific Flow, and [Get Run log](./logs/get-run-log.md) returns the full log entries for an individual run.

<br/>

#### Built-in types
Reference documentation for the types used by actions across Flow. [DeltaSet](./built-in-types/deltaset.md) represents a collection of inserts, updates, and deletes that can be applied to a database, file, or service — used by Power BI writeback triggers and the matching Save DeltaSet actions. [HttpResponse](./built-in-types/http-response.md) represents the response from an HTTP API call, including the data, status code, and error information. [IVectorSearchResult](./built-in-types/ai/i-vector-search-result.md) represents the result of a vector similarity search and can be passed directly to a chat completion action as context.

<br/>

#### Data Analysis
APIs for manipulating data inside a [Function](./../actions/built-in/function.md) action — see [Data Analysis](./data-analysis/overview.md) for the full reference, including DataTableTransformer for reshaping DataTables and JsonDataReader for flattening JSON into rows.
