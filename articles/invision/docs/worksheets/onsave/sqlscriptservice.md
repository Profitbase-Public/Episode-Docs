
# SQL Script Service

Use the SqlScriptService to run a SQL Script when a Worksheet or Table is saved.

<br/>

**Example** 

>calling a SqlScriptService method:



    // This method is called after the changes have been saved to the database.
    // The operationType specifies whether the rows was inserted, updated or deleted.

    protected override void OnAfterSave(IEnumerable<CudRecord> rows, TableOperationType operationType)
    {   
        base.OnAfterSave(rows, operationType);

        //Executes query after save has been done, returns the amount of rows affected
        this.SqlScriptService.ExecuteNonQueryById("24e871f8-cd46-4c51-9a46-cbf7eec66bff");
    }

    // This method is called at the end of the commit process.
    // The data parameters contains the rows that was deleted, updated or isserted.

    protected override void OnEndSave(CudDataPackage data)
    {
        base.OnEndSave(data);

        // When method is called returns the first value of the result set.
        this.SqlScriptService.ExecuteScalarById<int>("63aa2021-4092-48d1-ba8f-975aeecb5202");
    }


<br/>

## SqlScriptService methods
<br/>

**ExecuteNonQueryById**

> Executes a SQL Script, specified by its object id, and returns the number of rows affected by the query.
>
    public int ExecuteNonQueryById(string scriptId, 
    Dictionary<string, object> paramValues = null)

<br/>

**ExecuteNonQueryByName**

> Executes a SQL Script, specified by its object name , and returns the number of rows affected by the query.
>
    public int ExecuteNonQueryByName(string scriptName, 
    Dictionary<string, object> paramValues = null)

<br/>

**ExecuteScalarById**

> Executes a SQL Script, specified by its object id, and returns the value in the first column in the first row from the result set. 

 
> [!NOTE]
> The SQL Script must be configured to return data.



    public T ExecuteScalarById<T>(string scriptId, 
    Dictionary<string, object> paramValues = null)

<br/>

**ExecuteScalarByName**

> Executes a SQL Script, specified by its object name, and returns the value in the first column in the first row from the result set. 
 
> [!NOTE]
> The SQL Script must be configured to return data.


    public T ExecuteScalarByName<T>(string scriptName, 
    Dictionary<string, object> paramValues = null)

<br/>

**LoadById**

> Executes a SQL Script, specified by its object id, and returns the data set from the query. 
 
> [!NOTE]
> The SQL Script must be configured to return data.
>
    public DataTable LoadById(string scriptId, 
    Dictionary<string, object> paramValues = null)

<br/>

**LoadByName**

> Executes a SQL Script, specified by its object Name, and returns the data set from the query. 
 
> [!NOTE]
> The SQL Script must be configured to return data.


    public DataTable LoadByName(string scriptName, 
    Dictionary<string, object> paramValues = null)
