
# SQL Reports

InVision supports line and chart [reports](../../sqlreports.md), and a single report can toggle between line and chart display mode.

Reports are created by defining one or more data sets using [SQL queries](../../sqlreports/dataqueries.md) and then mapping those data sets to chart and table reports.

InVision reports can read data from any SQL source, so you can use reports to view data from any SQL Server database in addition to the InVision databases.
<br/>

![pic](https://profitbasedocs.blob.core.windows.net/images/sqlScr.png)


## Actions
<br/>

### LoadData

Instructs the component to load data. By default, this will load data from the InVision data model based on the configuration. If, however, you want to load data to the SQL Report using Flow instead, you can call `UseFlow(...)` in Load Data instructions.   
Read more about calling [UseFlow here](../programmingmodel/instructions/use-flow.md).

<br/>

### Execute Expression

Enables changing the caption of the report dynamically by calling SetCaption(…), SetCaptionSuffix(…) and ResetCaption(). 

<br/>

## Events
<br/>

### Cell Action Link Tapped

This event is raised when an action link cell in a table report is clicked. The payload of the event is the name of the column that was clicked and the row object.

### Cell Icon Clicked

This event is raised when a [clickable cell icon](../../tables/columnproperties/cellicon.md) returned by a cell renderer is clicked. The payload of the event is the row object, the name of the column, the cell value and the name of the icon (`IconName`). See [The Cell Icon Clicked event](../../tables/columnproperties/cellicon.md#the-cell-icon-clicked-event) for how to read them in JavaScript and Eaze.

### Custom Context Menu Action Executed

This event is raised when a custom context menu action is clicked. The payload of the event is the name of the action that was clicked, the column and the row object that the cell belongs to.

 ### @Event properties

 **Data** – Contains a reference to the data (row) object that is rendered by the clicked spreadsheet cell  
 **Sender.ColumnName** – The name of the column that was clicked  
 **Sender.ActionName** – The name of the custom context menu item action  
 **Selection.Rows** – A collection of [SelectedRow](#selectedrow-properties) objects  
 **Selection.Cells** – A collection of [SelectedCell](#selectedcell-properties) objects


**Example**  
Show a toast notification displaying the name of the action that was executed, and the value of the ProductID of the row that was clicked.

```javascript
ShowToastNotification(@Event.Sender.ActionName + " was executed", "Product Id is " + @Event.Data.ProductID);
```



#### SelectedRow properties

| Name     | Description                  |
|----------|-------------------------------|
| RowData  | The data object of the selected row, containing one property pr column |
| Columns  | The column names of the selected. The column names map directly to the property (field) names or the RowData object. | 


#### SelectedCell properties 

| Name     | Description                  |
|----------|-------------------------------|
| RowData  | The data object of the selected row, containing one property pr column. |
| Column   | The column name of the selected cell. For example, you can access the value of the first selected cell using `Event.Selection.Cells[0].RowData[Event.Selection.Cells[0].Column]`. |



### ChartInteractionEvent

This event can be raised from a Highchart event handler, for example the click event of a point. To raise this event, you need to add an event handler to the chart component that you want to raise that event from, and then raise the ChartInteraction event.

**Example**

To raise the ChartInteraction event when a point is clicked, add a JavaScript library to your solution and generate a default script. This will create a myUtil function object. 
Add the following function to the myUtil function object:

```javascript
    myUtil.onPointClick = function(){				
        this.series.chart.componentContext.eventDispatcher.raiseEvent  
        ('ChartInteraction', {x : this.x, y : this.y},  
        this.series.chart.componentContext.componentId);
    };
```

The second argument is the payload you want to pass along as the event argument.  

Then, in the Property editor, find the clicked event of the point of the series, and add a reference to the function with the **func::** prefix.
If your solution is called Hypotesia, and the JavaScript Solution object is called Chart Events, the correct pointer name is  
```javascript
        func::hypotesia.chartevents.myUtil.onPointClick
```

To display the x-value of the clicked series in a toast notification, add an ExecuteExpression action to the Workbook with the following expression:
```javascript
ShowToastNotification("Point clicked", "X value point is " + @Event.Data.x);
```

<br/>


![func](https://profitbasedocs.blob.core.windows.net/images/func.png "func")

<br/>

### Handling the ChartInteractionEvent from built-in event publishers

At the moment, InVision has a built-in function (**pb.chart.series.point.events.onClick**) that can be set to raise the **ChartInteractionEvent** when a series point is clicked. The function raises the **ChartInteractionEvent** with a **payload object** containing the following properties:

**X**  
>The x-value of the clicked point  

**Y**  
>The y value of the clicked point  

**low**  
>The low value of the clicked point (for range series).

**high**  
>The high value of the clicked point (for range series).

**seriesName**  
>The name of the series that the point belongs to.

**data**  
>The data object of the clicked point. The data object is the entire data row that the point is rendered from.

When handing the ChartInteractionEvent in a Workbook, the payload object is exposed through the **@Event.Data** property, so to get the **y** value of the clicked point in a Workbook event handler, you would write **@Event.Data.y.**

<br/>

## See Also
* [SQL Reports](../../sqlreports.md)

## Videos 

* [SQL Reports](../../../videos/sqlreports.md)
* [ChartInteractionEvent](https://profitbasedocs.blob.core.windows.net/videos/SQL%20Reports%20-%20ChartInteractionEvent.mp4)