# Azure Application Insights overview

Flow includes built-in support for sending telemetry to [Azure Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview), so you can log diagnostic messages and exceptions from your flows and review them alongside the rest of your application telemetry.

To use the Track action, you first need an [Azure Application Insights connection](./connection.md) configured with a connection string from your Application Insights resource in the [Azure Portal](https://portal.azure.com).

<br/>

## Explore

#### Connection
Set up the connection used by every Application Insights action. Uses the Application Insights connection string (instrumentation key, ingestion endpoint, and live endpoint) copied from the Azure Portal.  
[Read more](./connection.md)

<br/>

#### Track
Sends a diagnostics entry to Application Insights with a message, optional custom properties, severity level (Verbose, Information, Warning, Error, Critical), and an optional exception. Typically used in a Catch path to log errors, but can also be used for general diagnostics throughout a flow.  
[Read more](./track.md)
