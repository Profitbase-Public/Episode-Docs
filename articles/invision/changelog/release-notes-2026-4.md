# InVision 2026.3 release notes

## AI: Semantic model for Budgeting and Forecasting
The InVision platform now ships with a built-in extension for the Budgeting and Forecasting solution that generates the semantic model for report data (KPIs) so AI tools like Claude and Copilot can talk to financal planning data through MCP for data exploration and analysis. The extension automatically applies the security policies already defined in the solution to the semantic model data, ensuring that AI tool users only see what they are allowed to.

The system automatically updates the security policies whenever permissions are changed, and when users are added or removed. 
After financial calculations has completed, new actuals has been loaded from ERP systems, and when the AI configuration for KPIs have changed, developers need to make sure to have hooked up an [Execute Profitbase B&F Semantic Model operation](../../flow/actions/profitbase-invision/epm/execute-semantic-model-operation.md) to their dataflows so the semantic model data gets updated accordingly. End users are not required to do anything.

![img](/images/flow/semantic-layer-mcp-claude-report.png)

<br/>

## KPI card enhancements

#### Filter binding
KPI cards can now be bound to filters in the Workbook Designer, meaning developers no longer need to pass arguments programatically using `SetParamValue(...)` when loading data. This unifies the KPI development experience with the Worksheet, Table and SQL Report way of doing things and reduces the need for script commands.  

![img](/images/invision/release-note-2026-6-kpi-filter.png)

<br/>

#### Programmatic configuration (advanced)
For advanced scenarios, KPI cards can now be configured dynamically (at runtime) to render conditionally based on business logic and state in Workbooks - for example variables and in-memory datasets already loaded into the browser app.  

![img](/images/invision/release-notes-2026-4-kpi.png)

<br/>

**Example - re-use in-memory dataset**  
Suppose your Workbook contains a tabular Sales report, and you want KPI cards to display data from the same dataset. To avoid a server roundtrip, you can now simply reference the dataset already loaded by the Sales Report and build the KPI card data.

```javascript
function getConfigOverride(args, app){
  // Use the app.datasets API to get a dataset already loaded to the Workbook.
  // app.datasets.getNames() to list the names of all in-memory datasets in the Workbook.
  const kpiDataTable = app.datasets.get('Sales Report Data');
  let sum = 0;
  for(const row of kpiDataTable.length){
      sum += row.Amount;
  }

  return {
      value: sum
  }
}
```

**Example - use variables and custom business logic**  
Another example shows how you can use Workbook variables (`app.variables`) and custom business logic to programatically define the KPI card data.  

```javascript
function getConfigOverride(args, app){
  let pctComplete = args.row.PctComplete;
  // In case we return a non-numeric value (string) from the database, convert it
  if(!app.lib.numerics.isNumber(pctComplete)){
    pctComplete = app.lib.numerics.convertToNumber(pctComplete);
  }
  const initialThreshold = app.variables.initialThreshold ?? 25;
  let status = 'halfway'
  if(pctComplete < initialThreshold){
    status = 'initial'
  }else if(pctComplete >= 95){
    status = 'complete'
  }

  return {
    status
  }
}
```

<br/>

## Table Explorer enhancements

#### Flags and Data modified event


#### Other enhancements
- **Dynamic context scoping**: You can now programatically set the context scope by calling `SetContextFilter(...)` in the Load Data action of the Table Explorer. You'd typically do this to show only tables that are related to a specific business domain or process, for example `Sales` or `Forecast`.
- **Automatic grouping**: The Table Explorer now automatically groups tables by default if there are more than 2 tags and 10 tables displayed in the list.

<br/>

## APIs (Advanced)


## Coming soon...
For the past 4 months, we've been hard at work porting the InVision Designer from a legacy desktop application to a modern web application, meaning you soon don't have to install anything on your local computer to build InVision solutions and product extensions for the Budgeting and Forecasting solution. In addition to simplyfing our deployment model and shipping faster, the new browser-based tool is built on modern technologies that enables us to provide far better tooling for you to build solutions faster and maintain them more easily. Stay tuned!