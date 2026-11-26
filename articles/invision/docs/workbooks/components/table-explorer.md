# Table Explorer

The Table Explorer lets developers group tables by business domains so users can quickly discover and edit related data in a single UI instead of switching between multiple tabs, pages or Workbooks.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_web_purpose.png)

<br/>

## Getting started
To display a Table in a Table Explorer
1) Open a [Table](../../tables/index.md) for edit in the InVision Designer. 
2) Switch to the **Listing** tab.
3) Check **Display in Table Explorer(s)**
4) Add a Table Explorer component to a Workbook (in the same Solution)

<br/>

## Properties
| Name           |  Description                                     |
|----------------|--------------------------------------------------|
| Context filter  | Use this property to specify which Tables to display in the Table Explorer. Only Tables with at least one matching Context string is displayed. Use semicolons (;) to define multiple filter values. <br/>For example, `Forecast; HR` will display tables having a Context strings with `Forecast` OR `HR` tokens. You can also set the context filter in the Load Data action by calling the `SetContextFilter(...)` function. If no context filter is specified, all Tables marked as "Display in Table Explorer(s)" in the solution will be displayed. Read more about context filtering here: [Context and context filtering](../../tableexplorer/howto/context.md) |
| Default grouping | Specifies whether the tables are listed or grouped by tags when the Table Explorer opens.<br/>- **Automatic** (default): groups by tags when there are more than 2 tags and more than 10 tables.<br/>- **None**: lists the tables.<br/>- **By tags**: groups the tables by tags. If none of the tables have tags, they are listed instead.<br/>Users can still switch between the two views with the **Group by Tags** toggle. This property is only available in the web-based InVision Designer. |

<br/>

## Display in Workbook
To display a Table Explorer in a Workbook, select it from the toolbox. By default, it will fill the entire page, but you can change this from the Properties panel if you want to.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_workbook_page_n.png)

<br/>

Run the **Load Data**-action of the Table Explorer to initialize the UI.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_workbook_interaction_n.png)

<br/>

## Events
| Name               | Description               | Event args  |
|--------------------|---------------------------|-------------|
| Data modified      | Raised when one of the listed tables are edited by a user.| [Data modified event args](#data-modified-event-args) |

#### Data modified event args
```json
{
   "tableName": string,
   "objectId": string,
   "tags": string,
   "flags": string
}
```

**Example**
Example event handler for the Table Explorer Data modified event
```javascript
if(Event.Data.flags?.split(';')?.contains('inventory')){
    this.app.variables._state.isInventoryEdited = true;
}
```