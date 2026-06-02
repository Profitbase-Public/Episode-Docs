#	Source Mapping

In this **Example** we use Data Store that we created [here](creatingschema.md).

1.	Go to the “Source Mapping” tab.
2.	Click on the “Add Source” button.  
3.	Find the Fact table “HistProductSales” in the Item Picker pop up and click “Ok”.


**Notice** that the data in the HistProductSales table is not in the same format as the data store we just created. One alternative is to use the SQL Transform field to write SQL code that transform your data. Another option is to create a view and use the view as your data source. 

You can copy the SQL statement below into the SQL Transform window. We do this to create a column with margin % based on the data in the fact table. 

```
SELECT 
[DepartmentID],
[ProductID],
SUM([Amount]) AS HistSalesLY,
(1-(SUM([Amount])/SUM(COGS)))*-1 AS MarginPCT 

FROM @Object[HistProductSales, fact].DbObjectName
WHERE [DatasetID] = 'Actual' and YEAR(Transdate) = '2009'
Group BY 
[DepartmentID],
[ProductID]
```

4.	Paste the SQL query into the SQL Transform field
5.	Click “Save” or Ctrl + S
6.	Navigate to the “Column Bindings” area

![Source Mapping](https://profitbasedocs.blob.core.windows.net/images/sourcemapping1.png)

7.	Map your Source columns to your Data Store columns or use the “Auto Bind” function. Your mapping should look like this: 

![Source Mapping](https://profitbasedocs.blob.core.windows.net/images/sourcemapping2.png)


8.	Click “Save” or Ctrl + S
9.	Click on the “Add Source” button. Select the “Department” Dimension in the Item Picker pop-up and click “Ok”
10.	Use the “column Bindings” function to map the Dimension column DepartmentID against Data Store column DepartmentID. 
Make sure the binding type is set to “Relationship”.
11.	Repeat step 9 and 10 for the “Product” Dimension
12.	Click “Save” or Ctrl + S   
13.	Right click on your Data Store “Product Sales” in the Solution Explorer pane on the right-hand side and select “Reload Data”. Click “Yes” in the Reload Data pop-up.
This uses the query you entered in the SQL Transform field to load the selected data from the fact table into the data store. 
14.	When the “Reload data” dataflow is completed close the Data Flow execution dialog box. 
15.	Right click on your Data Store “Product Sales” and select “Browse Data” to explore the content of your Data Store

<br/>

## Videos
* [Data Stores](../../videos/datastores.md)
* [Basics - Creating Schema and Source Mapping](https://profitbasedocs.blob.core.windows.net/videos/DS%20Basic%20-Creating%20Schema%20and%20source%20maping.mp4)
