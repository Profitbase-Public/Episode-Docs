
# Table View

[Table View](../../tableview.md) is used for displaying data in a read-only data grid. Compared to spreadsheet tables ([Worksheet](../../worksheets.md), [Table](../../tables.md), and [SQL Report](../../sqlreports.md)) which are used for editing data, Table View is used for displaying data in a nicely formatted way.

The component has built-in features for selection, sorting, filtering, formatting, and custom renderers.

[How to create a Table View object](tableview.md)

<br/>

![TbV](https://profitbasedocs.blob.core.windows.net/images/TbView1.png)

<br/>


## Actions

<br/>

#### Load Data

Call this action to load or refresh the data displayed in the Table View component. By default, this will load data from the InVision data model based on the TableView configuration (SQL query). If, however, you want to load data to the Table View using Flow instead, you can call `UseFlow(...)` in Load Data instructions.   
Read more about calling [UseFlow here](../programmingmodel/instructions/use-flow.md).


<br/>


## Events

#### Cell Action Link Tapped

This event is raised when the user clicks a cell in an Action Link column. The event args has the following format:

##### Event args format

```json
{
    "Data": {
        "Property1": "",
        "Property2": "",
        …        
    },
    "Sender": {
        "ColumnName": "name of the column of the clicked cell",
        "ActionName": "ActionLink"
    },
    "Selection": {
        "Rows": [],
        "Cells": []
    }
}
```

> [!NOTE]
> The `Data` property contains the fields and values of the clicked row, meaning the Data object will have different formats for every Table View, depending on how the query is defined.

##### Example

The example below shows how to use the `Event args` in an action handler

# [JavaScript](#tab/cell-action-link-tapped-javascript)
```javascript
// Get the customer id of the clicked row
_state.customerId = Event.Data.CustomerId;

// Check which column was clicked
_state.columnClicked = Event.Sender.ColumnName;
```
# [Eaze](#tab/cell-action-link-tapped-eaze)

```javascript
// Get the customer id of the clicked row
_state.customerId = @Event.Data.CustomerId;

// Check which column was clicked
_state.columnClicked = @Event.Sender.ColumnName;
```
---

#### Cell Icon Clicked

This event is raised when the user clicks a [clickable cell icon](../../tables/columnproperties/cellicon.md) returned by a column Renderer. The event args has the following format:

##### Event args format

```json
{
    "Data": {
        "Property1": "",
        "Property2": "",
        …
    },
    "Sender": {
        "ColumnName": "name of the column of the clicked cell"
    },
    "CellValue": "value of the clicked cell",
    "IconName": "name of the clicked icon, for example popup"
}
```

##### Example

# [JavaScript](#tab/cell-icon-clicked-javascript)
```javascript
// Get the customer id of the clicked row
_state.customerId = Event.Data.CustomerId;

// Check which icon was clicked
_state.clickedIcon = Event.IconName;
```
# [Eaze](#tab/cell-icon-clicked-eaze)

```javascript
// Get the customer id of the clicked row
_state.customerId = @Event.Data.CustomerId;

// Check which icon was clicked
_state.clickedIcon = @Event.IconName;
```
---

#### Custom Context Menu Action Executed

This event is raised when a user clicks a context menu item. The event args has the following format:

##### Event args

```json
{
    "Data": {
        "Property1": "",
        "Property2": "",
        …
    },
    "Sender": {
        "ColumnName": "name of the column of the clicked cell",
        "ActionName": "name of the context menu item action"
    },
    "Selection": {
        "Rows": [
            {
                "RowData": {
                    "Property1": "",
                    "Property2": "",
                    …                   
                },
                "Columns": [
                    "Property1",
                    "Property2",
                    …
                ]
            }
        ],
        "Cells": [
            {
                "RowData": "cell value",
                "Column": "Property1"
            },
            {
                "RowData": "cell value",
                "Column": "Property2"
            }, 
        ]
    }
}

```

> [!NOTE]
> The `Data` property contains the fields and values of the clicked row. The Selection property contains the fields and values of the currently selected rows. These objects will be different for each Table View instance, depending on how the data source is defined.

The example below shows how to use the `Event args` in an action handler

# [JavaScript](#tab/custom-context-menu-action-javascript)
```javascript
// Get the customer id of row that the context menu action was executed on
_state.customerId = Event.Data.CustomerId;

// Check which column was clicked
_state.columnClicked = Event.Sender.ColumnName;

// Check the action name if you have defined multiple context menu actions for the same column
_state.actionName = Event.Sender.ActionName;

// Loop the selected rows and summarize the amount field
let sum = 0
for(const row of Event.Selection.Rows){
    sum += row.RowData.amount;
}

```
# [Eaze](#tab/custom-context-menu-action-eaze)

```javascript
// Get the customer id of row that the context menu action was executed on
_state.customerId = @Event.Data.CustomerId;

// Check which column was clicked
_state.columnClicked = @Event.Sender.ColumnName;

// Check the action name if you have defined multiple context menu actions for the same column
_state.actionName = @Event.Sender.ActionName;

```


---

#### Custom

This event can be raised from custom renderers, for example if you render clickable content in cells. 
The Event args contains a Data object which is sent from the renderer. 


##### Example
The custom renderer raises a custom event passing an object with a key and value property.

```javascript
function(eventArgs, services){  
    btn.onclick = () => services.eventDispatcher.raiseCustomEvent({key: 'hello', value: 'world'});    
}

```

The Event args will contain the following data:

```json
{
    Data: {
        key: "hello",
        value: "world"
     }
}
```


<br/>

#### Selection Changed

This event is raised when the row selection is changed by the user, for example when a user selects or deselects a row. 
The Event args contains data with the following format:

##### Event args

```json
{
    "Data": {
        "SelectedItems": [{
                "Property1": "",
                "Property2": "",
            }
        ],
        "ChangedItems": [{
                "Property1": "",
                "Property2": ""
            }
        ],
        "Action": "select” | “deselect"
    }
}
```

> [!NOTE]
> The “Data.SelectedItems and Data.ChangedItems” objects contain the fields and values of the clicked selected rows, so it will be different for each Table View instance, depending on how the data source is defined.


<br/>

The `Action` property specifies whether the user **selected** or **deselected** a row. Valid values are `select` and `deselect`.
The `SelectedItems` contains the currently selected items.  
The `ChangedItems` contains the items which were affected by the last change (select or deselect).
<br/>

The example below shows how to use the `Event args` in an action handler

# [JavaScript](#tab/selection-changed-javascript)
```javascript

// Loop the selected rows and summarize the amount field
let sum = 0
for(const row of Event.Data.SelectedItems){
    sum += row.amount;
}

```
# [Eaze](#tab/selection-changed-eaze)

```javascript
// Store the selected items in the global state
_state.selectedItems = @Event.Data.SelectedItems;

```
---

#### Row Clicked

This event is raised when the user clicks on a row. The Event args contains data on the following format:

#### Event args

```json

{
    "Data": {
        "RowData": {
            "Property1": "",
            "Property2": "",
             …            
        },
        "SelectedItems": [{
                "Property1": "",
                "Property2": "",
            }
        ],
        "ChangedItems": [{
                "Property1": "",
                "Property2": ""
            }
        ],
    }
}

```

The `RowData` property is the row that was clicked.  
The `SelectedItems` contains the items that are currently selected. Note! This does NOT include the clicked row if it was not alredy selected when the click occurred.  

> [!NOTE]
> The `Data.RowData` property contains the fields and values of the clicked row, so it will be different for each Table View instance, depending on how the data source is defined.

<br/>

### Videos

* [Table View](../../../videos/tableview.md)
* [Table View - Basics](https://profitbasedocs.blob.core.windows.net/videos/Table%20View%20-%20Basics.mp4)
* [Table View - Macro Expansion](https://profitbasedocs.blob.core.windows.net/videos/Table%20View%20-%20Macro%20Expansion.mp4)
* [Table View - Custom Cell Renderer](https://profitbasedocs.blob.core.windows.net/videos/Table%20View%20-%20Custom%20Cell%20Renderer.mp4)
* [Table View - Declaring Filter](https://profitbasedocs.blob.core.windows.net/videos/Table%20View%20-%20Declaring%20Filter.mp4)

