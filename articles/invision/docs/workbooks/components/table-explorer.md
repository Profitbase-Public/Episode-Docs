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

<br/>

## Display in Workbook
To display a Table Explorer in a Workbook, drag and drop it from the toolbox. By default, it will fill the entire page, but you can change this from the Properties panel if you want to.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_workbook_page.png)

<br/>

Run the **Load Data**-action of the Table Explorer to initialize the UI.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_workbook_interaction.png)

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