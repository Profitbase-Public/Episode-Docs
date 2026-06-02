
# Profitbase.Data.SqlCommand functions

`$result = [Profitbase.Data.SqlCommand]::ExecuteNonQuery($command [,$sqlParameters])`  
Executes a Transact-SQL statement against the solution database connection and returns the number of rows affected.

<br/>

`$dataTable = [Profitbase.Data.SqlCommand]::LoadTable($query [,$sqlParameters])`  
Executes a SQL query against the solution database connection and returns a data table object that contains the resulting columns and rows.

<br/>

`$objectResult = [Profitbase.Data.SqlCommand]::ExecuteScalar($query [,$sqlParameters])`  
Executes a SQL query and returns the first column of the first row in the result set returned by the query. Additional columns or rows are ignored.

<br/>

`$reader = [Profitbase.Data.SqlCommand]::ExecuteReader($query [,$sqlParameters])`  
Executes a SQL query against the solution database connection and returns a SqlDataReader object that can be used to read field and row data.  
> [!NOTE]
> The SqlDataReader object should always be closed after processing to avoid leaving database connection open longer than necessary.

<br/>

**Examples**

```powershell
[Profitbase.Data.SqlCommand]::LoadTable("SELECT * FROM [dbo].[pbInvision_PowerShellLog]")

[Profitbase.Data.SqlCommand]::ExecuteNonQuery("DELETE TOP (10) FROM [dbo].[pbInvision_PowerShellLog]")

[Profitbase.Data.SqlCommand]::ExecuteScalar("SELECT COUNT(*) FROM [dbo].[pbInvision_PowerShellLog]")


$reader = [Profitbase.Data.SqlCommand]::ExecuteReader("SELECT * FROM [dbo].[pbInvision_PowerShellLog]")
while ($reader.Read()) {
    Write-Output $reader["Message"]
}
$reader.Close()  # Closes reader and connection
```

<br/>

**Examples**

This example demonstrates how to use the ExecuteReader method of the SqlCommand class to execute a SQL query with parameters.

![pic](https://profitbasedocs.blob.core.windows.net/images/psExDi.png)

<br/>

**Examples**

This example demonstrates how to use the ExecuteReader method of the SqlCommand class to execute a SQL query the dictionary API.

```powershell
$query = "SELECT * FROM [dbo].[Customers] WHERE TimeStamp > @TimeStamp AND Level = @Level AND Id = @Id"

$dictionary = New-Object System.Collections.Generic.Dictionary"[String, Object]";

$dictionary.Add("@Id", 5023);

$dictionary.Add("@TimeStamp",  [datetime]::new(2023, 02, 10, 02, 05, 56));

$dictionary.Add("@Level", "Mid");

$reader = [Profitbase.Data.SqlCommand]::ExecuteReader($query, $dictionary)

while ($reader.Read()) {
    Write-Output $reader["Message"]
}

$reader.Close()

```

<br/>

## Videos

- [PowerShell](../../../videos/powershell.md)
- [PowerShell - Profitbase.Data.SqlCommand function example](https://profitbasedocs.blob.core.windows.net/videos/powershell%20-%20SQL%20Command.mp4)
- [PowerShell - SqlCommand API with SQL parameters and the dictionary API](https://profitbasedocs.blob.core.windows.net/videos/PS%20-%20SQLCommand.mp4)

<br/>
