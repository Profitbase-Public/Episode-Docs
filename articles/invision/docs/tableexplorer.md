# Table Explorer
The Table Explorer lets developers group tables by business domains so users can quickly discover and edit related data in a single UI instead of switching between multiple tabs, pages or Workbooks.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_web_purpose.png)

<br/>

## Getting started
To display a Table in a Table Explorer
1) Open a [table](../docs/tables.md) for edit in the InVision Designer. 
2) Switch to the **Listing** tab.
3) Check **Display in Table Explorer(s)**
4) Add a Table Explorer component to a Workbook (in the same Solution)

![pic](https://profitbasedocs.blob.core.windows.net/images/table-explorer-listing.png)

<br/>

## Properties
| Name                      | Type     | Description |
|---------------------------|----------|-------------|
| Display in Table Explorer(s) | Optional | Specifies whether the Table appears in Table Explorer(s) in Workbooks. |
| Context                   | Optional | A filter property used to categorize the table, enabling the table to appear in only select Table Explorers. For example, you can add a context string, `Budgeting and forecasting`, which makes it possible to display this table in only Table Explores having a matching Context filter (`Budgeting and forecasting`).<br/>This keyword acts as a possible filter value for the [Context filter](../docs/workbooks/components/table-explorer.md#properties). [See also  Context and context filtering](./tableexplorer/howto/context.md) |
| Short Description         | Optional | Shown as a subtitle for the table in the Table Explorer. This text can be localized using the associated "Text code". |
| Tags                      | Required | At least one tag is required. Tags help categorize and filter tables. Each tag supports localization via the provided "Text code". A table can have multiple tags. |
| Flags                     | Optional | When a user edits a cell in a table, a "Data Modified" event is raised on the Table Explorer. The event passes along the `Flags` property specified on Table. Developers can use this property to take conditional action in the Workbook when an edit occur. The name of the table is also passed along with the event, but the Flags property is often preferred as it can be used as a rule that spans multiple tables and not just one - similar to a policy.   |  

<br/>

<br/>

## How to's

#### Display in a Workbook
To display a Table Explorer in a Workbook, open the Workbook designer and drag - drop a Table Explorer component from the toolbox onto a page. <br/>
[Read more](../docs/workbooks/components/table-explorer.md)  

<br/>

#### Choosing which tables to display
Tables can be marked with a **context**, allowing the **Table Explorer** to filter and display only the relevant ones.<br/>
[Read more](./tableexplorer/howto/context.md)

<br/>

#### Searching for a table in a Table Explorer
You can quickly find a table by searching its name, short description, or by using the 'Search by Tags' option to filter by specific tags.<br/>
[Read more](./tableexplorer/howto/search.md)

<br/>

#### Editing data
You can edit the table data when it is opened. <br/>
[Read more](./tableexplorer/howto/edit.md)

<br/>

#### Filtering data
You can filter selected tables using the `SetParamValue()` function in the **Execute Expression** action for the **Table Explorer** component. <br/>
[Read more](./tableexplorer/howto/filterdata.md)

