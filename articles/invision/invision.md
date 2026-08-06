# Hypergene InVision 

_For business processes that doesn't fit in a standard system._  

Hypergene InVision is a low-code platform for building adaptable business applications on a modern technology stack. Users can leverage standard components to develop user interfaces and back-end services, significantly reducing both cost and time when implementing projects.

Applications are built using the InVision Designer and published as web applications within your organization — requiring minimal or no training to operate.

![Hypergene InVision — build robust and flexible business apps with low-code](/images/invision/invision_overview.png)

## Demo

Build a simple sales forecast application using InVision and Flow:

> [!Video https://www.youtube.com/watch?v=W9qlUNVekIg]

## Quick links

- [Documentation](./docs/index.md)
- [Videos](./videos.md)
- [Training cases](./trainingcase/index.md)
- [Hypergene InVision Demos](http://demo.profitbase.com/portal/#)
- [Changelog](./changelog.md)

## Key concepts

### Solutions

A Solution contains all data and configuration for your application: business logic, data stores, workbooks, worksheets, data flows, workflows, and more.

![InVision Solution overview](https://profitbasedocs.blob.core.windows.net/images/solution.png)

You can clone entire Solutions from the Designer start page. Cloning copies input data, permissions, and Data Flow schedules from the source.

> [!NOTE]
> Objects in cloned Solutions are not automatically added to the Data Pool after cloning. They will be added if the Data Pool Compound View is rebuilt. To prevent this, edit the cloned Solution and remove the Data Pool configuration from objects you don't want included.

When creating a new Solution, you must specify whether it should provide contents to Work Process blueprints. This is required upfront to avoid forgetting to set this property after deploying a Package.

### Designer

The Designer is the development tool for creating and modifying Solutions. It is a desktop application that you download locally to your PC or server.

![InVision Designer](https://profitbasedocs.blob.core.windows.net/images/designer.png)

### Workbooks

Workbooks are the user-facing interfaces for applications built on the InVision platform.

![InVision Workbook interface](https://profitbasedocs.blob.core.windows.net/images/client.png)

## Integration

InVision integrates with other business systems through REST APIs or direct connection to SQL data storage. Pre-built modules can be installed in minutes and customized to meet specific business requirements.

## Documentation reference

<div class="product-grid">
  <div class="product-card" style="flex-direction:column;padding:16px;">
    <h3 style="margin-bottom:8px;">Platform</h3>
    <p><a href="docs/systemsetup.md">System Setup</a></p>
    <p><a href="docs/accesscontrol.md">Access Control</a></p>
    <p><a href="docs/accessibility.md">Accessibility</a></p>
    <p><a href="docs/bestpractice/bestpractice.md">Best Practice</a></p>
    <p><a href="docs/homepage.md">Home Page</a></p>
    <p><a href="docs/translationsloc.md">Translations / Localization</a></p>
    <p><a href="docs/troubleshooting.md">Troubleshooting</a></p>
  </div>
  <div class="product-card" style="flex-direction:column;padding:16px;">
    <h3 style="margin-bottom:8px;">Data</h3>
    <p><a href="docs/datastores.md">Data Stores</a></p>
    <p><a href="docs/dataflows.md">Data Flows</a></p>
    <p><a href="docs/dataflowitems.md">Data Flow Items</a></p>
    <p><a href="docs/datapool.md">Data Pool</a></p>
    <p><a href="docs/dimensions/index.md">Dimensions</a></p>
    <p><a href="docs/tables.md">Tables</a></p>
    <p><a href="docs/tableview.md">Table View</a></p>
    <p><a href="docs/timeframe.md">Time Frame</a></p>
    <p><a href="docs/filestorage.md">File Storage</a></p>
  </div>
  <div class="product-card" style="flex-direction:column;padding:16px;">
    <h3 style="margin-bottom:8px;">Development</h3>
    <p><a href="docs/workbooks.md">Workbooks</a></p>
    <p><a href="docs/workbookmenu.md">Workbook Menu</a></p>
    <p><a href="docs/worksheets.md">Worksheets</a></p>
    <p><a href="docs/views.md">Views</a></p>
    <p><a href="docs/forms.md">Forms</a></p>
    <p><a href="docs/filters.md">Filters</a></p>
    <p><a href="docs/directives.md">Directives</a></p>
    <p><a href="docs/macros.md">Macros</a></p>
    <p><a href="docs/javascript.md">JavaScript</a></p>
    <p><a href="docs/powershell.md">PowerShell</a></p>
    <p><a href="docs/sqlreports.md">SQL Reports</a></p>
    <p><a href="docs/sqlscripts.md">SQL Scripts</a></p>
    <p><a href="docs/webfunctions.md">Web Functions</a></p>
    <p><a href="docs/webserviceapis.md">Web Service APIs</a></p>
    <p><a href="docs/systemviews.md">System Views</a></p>
    <p><a href="docs/workflow.md">Workflow</a></p>
    <p><a href="docs/workprocess.md">Work Process</a></p>
    <p><a href="docs/package.md">Packages</a></p>
  </div>
</div>