# GitHub overview

Flow includes built-in support for the GitHub REST API, letting you pull data from GitHub into your flows for two main scenarios: extracting issues and their comments for reporting, ticketing, or analysis, and reading repository content (files and directories) to deploy or copy files into other systems — such as deploying a Microsoft Fabric data pipeline from a Git repository, or syncing files from GitHub into a storage backend.

The actions in this category cover read operations only — Flow does not currently create or modify GitHub resources. Each action authenticates with a GitHub token configured per action via the **Authentication** property.

<br/>

## Explore

#### Working with issues
[Get issues](./get-issues.md) returns a list of issues from a repository, with options to filter by state (Open, Closed), date (**Issues since**), and to sort the result. Paging is configurable through **Number of pages to fetch** and **Issues per page** (1–200), and the issue body can be returned as Text, HTML, or Raw. [Get issue comments](./get-issue-comments.md) does the same for the comments on a specific issue. A typical pattern is to combine both — fetching all issues, then iterating through them to collect their comments and store everything in a SQL Server table for reporting.

<br/>

#### Browsing repository content
[For each Content Information](./for-each-content-info.md) iterates over the files and directories in a repository, returning a Content Information object with name, type, and URL for each item. Supports specifying a branch and starting path, filtering names with a regex, recursively including subdirectories, and choosing whether to include directories alongside files. Useful when you need to scan a repository for files matching a pattern before deciding what to do with them.

<br/>

#### Reading file contents
Once you've identified the file you want, two actions read its contents. [Read Content as stream](./read-content-as-stream.md) is preferred for performance and memory use; [Read Content as byte array](./read-content-as-byte-array.md) is required when the same content needs to be read more than once — for example, processing it and then archiving it. Both accept a repository owner, name, branch, and content path. The downloaded content is then typically passed to another action that consumes a stream or byte array — for example, uploading it to a file storage backend or feeding it to a deployment step.

<br/>

[!INCLUDE [](__videos.md)]
