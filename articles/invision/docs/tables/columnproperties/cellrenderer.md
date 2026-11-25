# Custom Cell Rendering

Use cell renderers to define custom rendering of cells in spreadsheets ([Tables](../../tables.md), [Worksheets](../../worksheets.md) and [SQL Reports](../../sqlreports.md)).  

A cell renderer is defined as a JavaScript function expression, or the name of a function defined in a Solution JavaScript.

using a format: 

 ```
 func::myscript.myfunction
 ```

A cell renderer must return a DOM object that can be displayed in a spreadsheet cell. If null or undefined is returned, the default (system) cell rendering will be used. 

Cell Renderers will only be executed for text, numeric and date columns. Cell Renderers will not execute for Dropdown, ActionLink and other special column types.
<br/>

> **Example**
>
> The following example shows how to write a custom cell renderer for rendering the cell contents as a button.

        function(eventArgs, services){
            var btn = document.createElement('button');

            btn.style.width = '100%';
            btn.style.height = '100%';
            btn.innerText = eventArgs.displayValue;

            return btn;
        }

---

## Function argument: eventArgs

The event argument passed to the cell renderer function has the following properties (note the camel casing):
<br/>

**eventArgs.displayValue**  
The formatted value of the cell. This is the value that would normally be displayed in the cell. Formatting is executed before the cell renderer, so you have access to both the raw value and the formatted value in your custom renderer.

**eventArgs.rowNum**  
The index of the row in the grid.

**eventArgs.colNum**  
The index of the column in the grid.

**eventArgs.prop**  
The property name of the rowData object that the cell is data bound to. This is usually the column name of the data set, for example “Amount”.

**eventArgs.rowData**  
The data object for the entire row.

---

## Function argument: services

The **services** argument provides access to the Workbook API for raising the Custom Event on spreadsheet components (Tabels, Worksheets and SQL Reports) from custom rendered cells. This makes it possible to trigger actions in a Workbook when a user performs some action in a custom rendered cell.  
<br/>

**services.eventDispatcher**  
Provides access to the eventDispatcher service that enables raising the Custom Event on the host component (Table, Worksheet or SQL Report)

> **Example**
>
> This example shows how to raise the "Custom" event of a Worksheet when a button in a custom rendered cell is clicked.

        function(eventArgs, services){
            var btn = document.createElement('button');

            btn.style.width = '100%';
            btn.style.height = '100%';
            btn.innerText = eventArgs.displayValue;

            btn.onclick = () => services.eventDispatcher.raiseCustomEvent({row : eventArgs.rowData});

            return btn;
        }



<br/>

## Clickable cell icons

A cell renderer can return a `<cell-icon>` element to show a built-in icon that works like a button and raises the **Cell Icon Clicked** event. See [Clickable Cell Icons](cellicon.md).

<br/>

## Videos
* [Worksheet](../../../videos/worksheet.md)
* [Custom Cell Rendering](https://profitbasedocs.blob.core.windows.net/videos/Spreadsheet%20-%20Column%20Properties%20-%20Custom%20Cell%20Renderer.mp4)
* [Custom Cell Rendering in Table View](https://profitbasedocs.blob.core.windows.net/videos/Table%20View%20-%20Custom%20Cell%20Renderer.mp4)