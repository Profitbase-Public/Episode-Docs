
# Input Configuration

<br/>

## Relationship

Specifies the column mapping used for determining which rows from the source table gets processed by the Spawner when the pipeline is run. In addition to this mapping, the Spawner Ruleset table may extend the filter by specifying a Condition Expression column containing a logical expression indicating whether rows should be spawned from on the source row or not. The rows from the source table not passing the filtering step are ignored by the component.
<br/>

## Computation Source Column(s)

Specifies the column(s) to process. For each source row passing the Relationship (and optionally Condition Expression) filter, the spawner will produce rows based on the Ruleset configuration. The number of rows generated for every source row depends on the number of matches found in the Ruleset table. Note! The columns defined in this list must be Time Frame Measure Columns unless the Distributer is enabled, in which case the transaction date is derived from the current batch of distributed transactions.
<br/>


**Properties:**

``Column Name``  
>Specifies the column to spawn transactions for, and write to the target.

``TypeId``  
>Specifies an identifier for the Computation Source Column that maps to a set of business rules in the Ruleset table. By default, the TypeId value is equal to the Column Name. Suppose you have two Computation Source Columns, Forecast1 and Forecast2, and you want to use the same set of business rules for both of them, you can specify the same TypeId value for both of them and use that TypeId value in the Type ID column of the Ruleset table.

``No Commit``  
>Specifies whether no rows should be generated for the column by the Spawner component. 

``Value Expression (SQL)``  
>SQL expression for computing the value of the column when it's read from the database, for example Amount \* 0.3. The resulting expression will be 
**SELECT [Expression] AS [Column Name],…**  
>**Example**
```
SELECT (Amount * 0.3) AS Amount ,…
```

``Processing Condition (C\#)``  
>Optional. A C\# expression returning a boolean value (true/false) used for specifying whether to process the current input row for the Computation Source Column. You can use the **Condition Expression** methods as part the Processing Condition expression.
>
>**Example 1**  
Suppose you only want to spawn transactions from a distributed row if the value of the distributed row is 5000, and DepartmentID for the input row does not equal "X-100", you can use the following expression: 
```
Value() > 5000 && DepartmentID != "X-100"
```
>
>**Example 2**  
Suppose you do not want to spawn transactions to dates in July and August, you can use the following expression: 
```
Month() != 7 && Month() != 8
```

<br/>


## Additional columns

Additional columns defines a list of columns that can be included in the output with the standard output. To output the additional columns, they need to be mapped to target columns in the **Additional Output** list of the Output configuration.
<br/>

**Properties:**

``Column Name``  
>Specifies the name of the additonal column to include in the output.


``Computed Column``  
>Allows specifying a custom column which does not exist in the source. When this feature is enabled, the Expression property must have a valid SQL statement which provides the value for the computed column.

``SQL Value Expression``  
>SQL expression for computing the value of the column when it's read from the database, for example substring (AccountID,0,2) The resulting expression will be **SELECT [expression] AS [Column Name],…**  
>Example 
```
SELECT (Amount * 0.3) AS Amount, …
```