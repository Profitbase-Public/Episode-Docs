
# Run SQL Scripts from Web Functions

## WebFunction base class APIs

<br/>

### SqlScriptService

The SqlScriptService is the API for executing SQL Scripts defined in your Solution and is available if your Web Function class inherits from the WebFunction base class.
<br/>

**Example calling a SqlScriptService method.**

_More examples are available in the Samples window in the InVision Desktop Designer_

    public async Task<IActionResult> Run(HttpRequest request)
    {
        //Create the script arguments from the request payload.
        //When called from a Workbook, the request payload is an object with properties set by calls to
        //for example: HttpPost("api/webfunctions/functionid", {"ItemId" : "myValue"});


        dynamic requestBodyObject = JObject.Parse(await request.Content.ReadAsStringAsync());
        var scriptArguments = new Dictionary<string,object>
        {
            {"@ItemID", requestBodyObject.ItemId.Value}
        };

        // Executing SQL Script that returns an integer and expects an @ItemID SQL parameter
        var value = await this.SqlScriptService.ExecuteScalarByIdAsync<int>("scriptId", scriptArguments);
    }

<br/>

### Methods

`ExecuteNonQueryByIdAsync`  
Asynchronusly executes a SQL Script, specified by its object id, and returns the number of rows affected by the query.

```csharp
Task<int> ExecuteNonQueryByIdAsync(string scriptId, Dictionary<string, object> paramValues = null)
```

<br/>

`ExecuteNonQueryByNameAsync`  
Asynchronusly executes a SQL Script, specified by its object name, and returns the number of rows affected by the query.

```csharp
Task<int> ExecuteNonQueryByNameAsync(string scriptName, Dictionary<string, object> paramValues = null)
```

<br/>

`ExecuteScalarByIdAsync`  
Asynchronusly executes a SQL Script, specified by its object id, and returns the value in the first column in the first row from the result set. 

> [!NOTE]
> The SQL Script must be configured to return data.

```csharp
Task<T> ExecuteScalarByIdAsync<T>(string scriptId,  Dictionary<string, object> paramValues = null)
```

<br/>

`ExecuteScalarByNameAsync`  
Asynchronusly executes a SQL Script, specified by its object name, and returns the value in the first column in the first row from the result set. 

> [!NOTE]
> The SQL Script must be configured to return data.

```csharp
Task<T> ExecuteScalarByNameAsync<T>(string scriptName, Dictionary<string,object> paramValues = null)
```

<br/>

`LoadByNameAsync`  
Asynchronusly executes a SQL Script, specified by its object name, and returns the data set from the query. 

> [!NOTE]
> The SQL Script must be configured to return data.

```csharp
Task<DataTable> LoadByNameAsync(string scriptName,  Dictionary<string, object> paramValues = null)
```

<br/>

`LoadByNameAsync`  
Asynchronusly executes a SQL Script, specified by its object id, and returns the data set from the query. 

> [!NOTE]
> The SQL Script must be configured to return data.

```csharp
Task<DataTable> LoadByIdAsync(string scriptId,  Dictionary<string, object> paramValues = null)
```
