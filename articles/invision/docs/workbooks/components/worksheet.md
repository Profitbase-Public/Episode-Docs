# Worksheet

<br/>

## Actions
<br/>

### Load Data
Use this action to load or refresh the table. By default, this will load data from the InVision data model based on the Worksheet configuration. If, however, you want to load data to the Worksheet using Flow instead, you can call `UseFlow(...)` in Load Data instructions.   
Read more about calling [UseFlow here](../programmingmodel/instructions/use-flow.md).

**Example - load data using Flow instead of InVision**  
In the Load Data action of the Worksheet, add this call to the Instructions script.  
The data will be loaded from a Flow returning a DataPackage.  

```javascript
UseFlow("Name of the Flow in the Solution");
```

<br/>

### Save Data
Use this action to save all of the changes to the database.

<br/>

### Send Data
Use this action to send the data in the Worksheet directly to Flow for processing, and optionally get back an updated data set. 
You will typically use this action instead of `Save Data` if you want to use Flow to apply custom, server-side business logic, on the data in the Worksheet. This typically involves advanced calculations requiring access to data stored in databases or other external systems such as files or HTTP APIs. 
If the Flow returns a [DataPackage](../../../../flow/actions/profitbase-invision/convert-datatable-to-datapackage.md), the data in the Worksheet is updated accordingly. 
To send data in the Worksheet to Flow, call the `UseFlow(...)` function in the `Send Data` instructions.  
Read more about calling [UseFlow here](../programmingmodel/instructions/use-flow.md).

**Example**  
```javascript
UseFlow("Update forecast", {
    environment: 'Production', // or 'Development' or 'Test'. Leave blank to use default
    data: {
        departmentId: "sales-NO",        
        version: "Jan-26-01"
    }
});
```

![img](/images/invision/send-data-use-flow.png)

<br/>

### Add Item
Use this action to add new row to the table.


<br/>

### ExecuteExpression
Executes one or more [instructions](../programmingmodel/instructions.md).


<br/>

### Unload Data
Unloads the data.

<br/>

### Validate Data
[Validates](../programmingmodel/instructions/validate.md) data. Run it if you want to run the Cell Validation rules before data is saved, and optionally prevent the save process for the particular Worksheet from executing if the validation fails.

```
Validate(terminateOnValidationFailed : boolean | Eaze expression)
```


<br/>

### Recalc
The Recalc action triggers the execution of the formulas script in the spreadsheet. You can call [SetDataModified(…)](../programmingmodel/instructions/setdatamodified.md) or [Refilter(...)](../programmingmodel/instructions/refilter.md) in Recalc actions.

<br/>


![calc](https://profitbasedocs.blob.core.windows.net/images/calculations.png)


<br/>

## Events
<br/>

### Data Modified
This event is raised when data in a table is modified. 
 
<br/>

### Cell Action Link Tapped
This event is raised when an action link cell in a table is clicked. The payload of the event is the name of the column that was clicked and the row object.

<br/>

### Custom Context Menu Action Executed
This event is raised when a (custom) Context Menu Action is clicked. The payload of the event is the name of the action that was clicked, the column and the row object that the cell belongs to.

### Extension Context Menu Action Executed
This event is raised when an Extension Context Menu Action is clicked. The payload of the event is the name of the action that was clicked, the column and the row object that the cell belongs to. 
The event handler will be preserved during Package upgrades, meaning customers / partners can use this feature to extend standard products like Planner with custom business logic.

<br/>

### Item Added
This event is raised when you add a new item to the table, for example, a new row. 

<br/>
