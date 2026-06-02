
# Input Configuration 

<br/>

## Relationship

Specifies the column mapping used for determining which distribution key is used for computing the value of resulting transactions. When a row from the source table does not match, the incoming value from the Computation Source Column is distributed evenly across the resulting transactions.
<br/>

## Computation Source Column(s)

Specifies the column(s) to unpivot (to a date-value pair) and write to the target. For each column in the list, the Distributer generates one row for every distribution key value. 

<br/>

**Properties:**
<br/>

``Column Name``  
Specifies the Time Frame Measure column to unpivot and write to the target.  

``Function``  
**Value \* Distribution Value**  
Will multiply the value of the Computation Source Colum with the corresponding distribution key value.  
**Value \* Distribution Value Share**  
Will multiply the value of the Computation Source column with the share of the corresponding distribution key value. For example if the distribution key consists of 3 values (P1 : 1.0, P2 : 4.0, P3 :5.0), then the share for P2 is 4.0/10.0.

``Type Id``  
Specifies an identifier for the Computation Source Column that maps to a set of business rules in the Ruleset table. By default, the TypeId value is equal to the Column Name. Suppose you have two Computation Source Columns, Forecast1 and Forecast2, and you want to use the same set of business rules for both of them, you can specify the same TypeId value for both of them and use that TypeId value in the Type ID column of the Ruleset table.

``From Date``  
Optional. An expression returning a Date specifying the lower bound of the distribution key used for distributing the value of the Computation Source Column. By specifying the From Date and To Date properties, only the factors of the distribution key within the specified range will be used for generating rows.

``To Date``  
Optional. An expression returning a Date specifying the upper bound of the distribution key used for distributing the value of the Computation Source Column. By specifying the From Date and To Date properties, only the factors of the distribution key within the specified range will be used for generating rows.

``No Commit``  
Specifies if no output row should be generated for the column by the Distributer.

``Enable Even Distribution Key Fallback``  
If this option is enabled, the Distributer will apply an even distribution to create transactions if no distribution key is found for the source row based on the Relationship, Type Id and Distribution Key Name configuration. The default value for this option is false.

``Value Expression (SQL)``  
SQL expression for computing the value of the column when it's read from the database. The resulting expression will be **SELECT [expression] AS [Column Name],…**  
**Example** 
```sql
SELECT (Amount * 0.3) AS Amount,…
```

``Processing Condition (C#)``  
Optional. A C# expression returning a boolean value (true / false) used for specifying whether to process the current input row for the Computation Source Column. You can use the *Condition Expression* methods as part the Processing Condition expression.  

**Example 1**  
Suppose you only want to distribute transactions from the Data Store if Distribution Value is more than 0.5, and DepartmentID for the input row does not equal "X-100", you can use the following expression:  

```csharp
CurrentDistributionValue > 0.5 && DepartmentID != "X-100"
```

**Example 2** Suppose you do not want to distribute transactions to dates in July and August, you can use the following expression: 
```csharp
Month() != 7 && Month() != 8
```