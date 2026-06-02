# Dropdown Selection

Specifies whether the column editor should be a dropdown editor. The configuration for setting up a Dropdown editor column depends on whether the column is a **standard column** or a **linked dimension column**.

<br/>

## Options
<br/>


  **Merge with Target Column Items**

  Enable this property in the rare cases when the Worksheet contains values in the column that cannot be found in the data source.

  **Enforce uniqueness**

  Enable this property to prevent the user from selecting the same value from the Dropdown editor for two different rows.

  **Distinct Values**

  Specifies that the Dropdown editor should only contain distinct values from the Dropdown data source.

  **Disable Access Control**

  Only applies to Dropdowns for linked dimension columns.
  Specifies that any Access Control rules defined for the dimension should be ignored when loading data to the Dropdown editor.



<br/>

### Setting up Dropdown for a linked dimension column

<br/>

> [!NOTE]
> (Only applies to Worksheets)
> When setting up Dropdown editing for a linked dimension column, you first need to ensure that the Data Store column that the  dimension is bound to in the star schema configuration is added to the Worksheet.

>[!NOTE]
>If there is no fact data source attached to the Data Store column, you need to ensure that the dimension binding type is set to "Relationship".*

Because the star schema binding configuration specifies the relationship between the dimension column and the Data Store column, the Data Store column added to the Worksheet will get its values updated when the user selects a value from the Dropdown editor containing dimension property values. 

To enable Dropdown editing of the column, check "Enable Dropdown Selection" in the property editor.


<br/>

### Setting up Dropdown for a standard column

<br/>

When setting up Dropdown editing of a standard column, you need to specify where the dropdown editor should get its values from, and optionally which other columns in the grid to update based on the selected value.

**Specify Data Source using @Object directive**

Allows specifying the Data Source for the dropdown editor using the @Object-directive. We use this property when setting up a Dropdown for resources that should be deployed as part of a template. Using the @Object-directive allows you to specify the Solution object name of the Dropdown data source instead of the materialized table name, which is unique for each object instance.

**Source Table**

Specifies the data source for the Dropdown editor. The data source can be a Data Store, Setting, Fact, Dimension or View.

**Source Value Member Column**

The column in the data source that should be used as the display member of the dropdown list.

**Target Column**

The column in the data set of the grid to have its value updated from *Source Column*.

**Secondary Update**

Using the Secondary Update option, you can set up the Dropdown editor to set the value of a second column in the grid when a value is selected.

**Source Display Member Column**

The column in the data source that contains the values to use for updating the second column in the grid.

**Target Column**

The second column of the grid to update when a value is selected in the Dropdown editor.


<br/>

### Custom Display Column Expression

  A SQL expression that returns the value to display in the Dropdown column.  

**Note** 
*When you apply a Custom Display Column Expression to a Dropdown column editor, you need to use Custom Query and apply the same expression to the corresponding column in the custom query.*



**Example**

Suppose you have a Worksheet with a linked dimension column and want to enable Dropdown editing of the column, but you want to display a computed value in the dropdown editor. The column name is ‘account167AccountID_Name’.

  **1)**	In the Dropdown editor configuration, specify the following expression
          
         SUBSTRING(@Context.ObjectAlias.AccountID_Name,0,2) 
         AS account167AccountID_Name 
  
  **2)**	From the Query tab in the Worksheet designer, enable "Use custom query to load data" and edit, Sync with Designer to get the standard query expression and edit the column expression for the joined dimension column.
  
     SELECT store.PBRowIdentity,  
     ISNULL(store.IsLocked,0)   
     AS IsLocked,  
     SUBSTRING(T0.[AccountID_Name],0,2)  
     AS [account167AccountID_Name]
     ,store.[AccountID]  
     AS [AccountID]  
     FROM [Store_Hypotesia_05182015125711] store  
     INNER JOIN [dbo].[DimLink_AccountWOarPFrSvNwgMF9XY3OC_account167]  
     T0 ON store.[AccountID] = T0.[AccountID]  
     COLLATE DATABASE_DEFAULT 
 


<br/>


### Filter

A SQL expression used in a WHERE clause to filter the values of the Dropdown editor.

**Example**
  
```sql
SUBSTRING(@Context.ObjectAlias.AccountID_Name,0,2) = '12'
```

You can use **custom parameters** as part of the filter expression, which enables the dropdown list to contain only a subset of the default items. To use this option, you need to use the [SetParamValue(…)](../../workbooks/programmingmodel/instructions/setparamvalue.md) and [ReloadDropDownItems(…)]() directives in the LoadData action of the Workbook component.


  **Example**
  
This example shows how to use a custom parameter to filter the available items in the dropdown editor. To pass a value to the @ItemID parameter, you must use the SetParamValue(…) and the @ReloadDropDownItems(…) instructions in the LoadData action of the Workbook component.

*The LoadData action instruction list must contain the following:*  
```javascript
SetParamValue("@ItemID",@Var[ItemID]);  
ReloadDropDownItems();
```

*The filter expression:*  
```
@Context.ObjectAlias.ItemID = @ItemID
```



<br/>


### Order By


  A SQL expression used for ordering the values of the Dropdown editor.

  **Example**
  
  ```sql
  LEFT(@Context.ObjectAlias.AccountID_Name,2)
  ```



<br/>

### See Also
* [Dropdown - Form Elements](../../forms/formelements/dropdown.md)
* [Dropdown - Control](../../forms/formschemas/controls/dropdown.md)


<br/>

## Videos


* [Dropdown Selection](https://profitbasedocs.blob.core.windows.net/videos/Worksheet%20-%20Dropdown%20Selection.mp4)
* [Dropdown](https://profitbasedocs.blob.core.windows.net/videos/Dropdown.mp4)