# InVision 2026.3 release notes

## KPI component
The new [KPI](../docs/kpi/index.md) component makes it easy for developers to add KPI visuals to Workbooks to create  dashboards with actionable insights. The KPI component has a flexible configuration system, which enables creating a wide range of different-looking KPI cards - from simple numeric values to KPIs with charts and images.

![img](https://profitbasedocs.blob.core.windows.net/images/kpi-card-workbook.png)

<br/>

## Toolbars for Worksheets and Tables
A long-awaited feature has finally arrived; built-in toolbars for Worksheets and Tables. This feature simplifies the job of creating Workbook pages with input tables because you no longer have to manually add a `Save` button to the page just to save data in a table. 
The toolbar also contains buttons for refreshing / reloading data, adding a new row, and exporting to Excel. 
Whether you want to enable the Toolbar, and which buttons to show, is configured in the Workbook Designer.

<br/>

## Workflow status report
We've replaced the old Workflow progress report with a new, modern dashboard containing KPIs and charts to show the current status of a Workflow.

![img](/images/invision/workflow-standard-report.png)

<br/>

## Possible breaking changes
**Datagrid updated to lastest version (Handsontable v17)**  
The latest version of the datagrid (used in Worksheets, Tables and SQL Reports) fixes a number of issues (for example copy-pasting JSON using keyboard shortcuts), but also introduces some possible breaking changes to existing solutions.  
- Cells are now a few pixels wider and higher, making tables easier to read and edit, but a little less compact. This may affect tables tailored to specific screen widths and heights to now display scrollbars.
- You can no longer use `only` CSS background images to hide (overlay) cell contents. If you want to show an image instead of the underlying cell value, you must use a `custom cell renderer` to render only the image. If you only apply a background image via CSS, both the image and text will be shown. 


**Removed support for LESS for theming and custom styling**  
We now only support standard CSS when defining custom styles and themes.  
This change may break custom solutions having used LESS functions in custom styles.
