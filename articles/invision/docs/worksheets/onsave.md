
# On Save

InVision provides a way to hook into the process of saving Worksheet and Setting data from the end user client. The hook lets you execute custom business logic such as data validation, modifying data, sending messages and so on.

The custom business logic can be written in C# and T-SQL, and is executed automatically if the feature is enabled.

Saving data is performed as a sequence of steps where rows scheduled for deletion is handled first, then updates, and finally new rows are added. You can write custom business logic that can execute before and after each one of these steps. The diagram below shows the pipeline being executed during a save process.

![On Save](https://profitbasedocs.blob.core.windows.net/images/worksheetsOnsave.jpg)


#### OnBeginSave(CudPackage data)

This function executes once before any data is saved. The argument passed to the function is and object of type CudPackage (Cud = create, update, delete) containing the rows to delete, update and insert. 

#### OnBeforeSave  
**(``IEnumerable<CudRecord>`` rows, TableOperationType operationType)**

This function executes once for each batch or rows (deletes, updates and inserts) before any data is saved.  
The first argument passed to the function is the collection of rows to delete, update or insert.  
The second argument passed to the function is the type of operation; Delete, Update or Insert. 

 #### OnAfterSave  
**(``IEnumerable<CudRecord>`` rows, TableOperationType operationType)**

This function executes once for each batch or rows (deletes, updates, inserts) after the data has been saved to the database.  
The first argument passed to the function is the collection of rows that was deleted, updated or inserted.  
The second argument passed to the function is the type of operation; Delete, Update or Insert.

#### OnEndSave(CudPackage data)

This function executes one time when the all the changes have been saved to the database. The argument passed to the function is and object of type CudPackage (Cud = create, update, delete) containing the rows that was deleted, updated and inserted.

#### Executing SQL Scripts from the main pipeline

SQL Scripts can be executed from the pipeline by calling <span style="color: blue;">SQL Execute functions</span>, passing the name of the <span style="color: blue;">name of the SQL scriptet</span> to execute. Any aruments to the script can be passed as the second parameter to the function. 



>**Example** – Executing a named SQL Script,"Script1", without parameters.
>
>protected override void OnBeginSave(CudDataPackage data)
>
        {
                …
                ExecuteNonQuery(Script1);
        }



>**Example** – Executing a SQL query without parameters
>
>protected override void OnBeginSave(CudDataPackage data)
>
    {
            …
            var vehicleId = "x";
            ExecuteNonQuery(string.Concat("INSERT INTO Table1 (VehicleID,Distance)  
            VALUES('", vehicleId, "', 300)"));
    }


#### Named SQL Scripts

If you want to execute sql scripts in the save pipeline, it is recommended to create Named SQL Scripts to keep the code in the main pipeline cleaner. When creating a Named SQL Script, you simply create a SQL script and assign a name to it. When you want to execute that script in the save pipeline, you simply provide the name of the script to the execute function instead of inlining the sql statement in the function. Named SQL Scripts are created using standard T-SQL syntax and optionally InVision <span style="color: blue;">direktives</span>. 

#### Executing business logic before save

You can execute custom business logic before data is saved by overriding the <span style="color: blue;">OnBeginSave</span> and <span style="color: blue;">OnBeforeSave</span> methods.

OnBeforeSave executes once pr batch type (Insert, Update and Delete), while OnBeginSave executes only once for the entire save process. If you override OnBeforeSave, you need to check the value of the **operationType** parameter to determine if the rows are inserted, updated or deleted. 

**Example**

This example shows how to can override the OnBeginSave method to execute a SQL query for each new row being inserted during a save process. The example also shows how to create a parameterized SQL query and pass arguments using a list of QueryParam objects.
```csharp
protected override void OnBeginSave(CudDataPackage data)
{
    if(data.Inserts != null)
    {
            foreach(var row in data.Inserts)
            {
                    ExecuteNonQuery("Update @Object[UpdateHistory].DbObjectName SET IsManuallyModified = 1                     WHERE ItemID = @ItemID AND RegionID = @RegionID", 
                    new QueryParam[]{ new QueryParam("@ItemID", row.GetValue<string>("ItemID")), new QueryParam("@RegionID",row.GetValue<string>("RegionID"))}, true);
            }
    }
}
```

#### Excecuting business logic after save

You can execute custom business logic after data has been saved by overriding the OnAfterSave or OnEndSave methods.

OnAfterSave executes once pr batch type (Insert, Update and Delete), while OnEndSave executes only once for the entire save process. If you override OnAfterSave, you need to check the value of the **operationType** parameter to determine if the rows were inserted, updated or deleted. 

**Example**

This example shows how to override the OnEndSave to execute a SQL query for each row that was updated during the save process. The example also shows how to create a parameterized SQL query and pass arguments using a list of QueryParam objects.

```csharp

protected override void OnEndSave(CudDataPackage data)
{
    if(data.Updates != null)
    {
        foreach(var row in data.Updates)
        {
                ExecuteNonQuery("Update @Object[UpdateHistory].DbObjectName SET IsManuallyModified = 1 WHERE AccountID = @AccountID", 
                new QueryParam[]{ new QueryParam("@AccountID", row.GetValue<string>("AccountID")) }, true);
        }
    }
}
```

#### QueryParam

Used for passing arguments to parameterized SQL queries or Named SQL Scripts.


**Constructors:**

Method  
``QueryParam(string paramName, object value)``  
Creates a new instance of this object.

**Properties:**

``ParamName``  
*System.String*  
The name of the parameter specified in the SQL expression.

``ParamValue``  
*System.Object*  
The value of the parameter.


#### TableOperationType

Enum describing the type of operation being executed.


**Values:**

``Insert``  
Rows are being inserted.

``Update``  
Rows are being updated.

``Delete``  
Rows are being deleted.


**Example**

This example shows to override OnAfterSave and examine the operationType parameter to determine if the rows being passed to the method will be inserted, updated or deleted.  

```csharp

protected override void OnAfterSave(IEnumerable<CudRecord> rows,   TableOperationType operationType)
{    
    base.OnAfterSave(rows, operationType);
    if(operationType == TableOperationType.Update)
    {
        // Implementation goes here
    }    
}

```

#### SQL Execute functions

___

**ExecuteNonQuery**

Executes a SQL expression and returns the number of rows affected. 

**Return value**

System.Int32

**Parameters:**

**sqlQuery**  
``System.String``  
A SQL expression or the name of a Named SQL Script.

**queryParamsString**  
``IEnumerable<QueryParam>``  
Optional. A list of arguments to pass if the SQL expression is parameterized.

**processDirectives**  
``System.Boolean``  
Optional. Default value = true. Specifies whether the SQL expression contains directives which needs to be processed.

**connectionString**  
``System.String``  
Optional. Specifies the connection string to use when executing the SQL expression.

___
**ExecuteScalar**

Executes the query, and returns the first column of the first row in the result set returned by the query. Additional columns or rows are ignored.

**Return value**

System.Object

**Parameters:**

**sqlQuery**  
``System.String``  
A SQL expression or the name of a Named SQL Script.

**queryParamsString**  
``IEnumerable<QueryParam>``  
Optional. A list of arguments to pass if the SQL expression is parameterized.

**processDirectives**  
``System.Boolean``  
Optional. Default value = true. Specifies whether the SQL expression contains directives which needs to be processed.

**connectionString**  
``System.String``  
Optional. Specifies the connection string to use when executing the SQL expression.


___

**ExecuteScalar < T >**  


Executes the query and returns the first column of the first row in the result set returned by the query. Additional columns or rows are ignored.

**Return value**

Objekt of type T

**Parameters**

See the non-generic version of <span style="color: blue;">ExecuteScalar</span>

**GetObjectConnectionString**

Returns the database connection string for an InVision database object (Data Store, Setting, Fact, Dimension or View) 

**Return value**

System.String

**Parameters:**

**objectName**  
``System.String ``  
The name of the Solution object (displayed in the Solution Explorer).

**objectType**  
``System.String ``  
Optional, but must be provided if there is more than one object in the Solution with the specified name.

<br/>

### See Also

- [API References](onsave/apireferences.md)
- [SQL Script Service](onsave/sqlscriptservice.md)
<br/>