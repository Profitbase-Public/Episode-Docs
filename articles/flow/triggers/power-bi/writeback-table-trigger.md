# Writeback trigger for the Profitbase Writeback Table

The trigger defines the starting point of a Flow that **stores data** from the [Profitbase Writeback Table](../../../PowerBI/writeback-table/overview.md) for Power BI.

The [Writeback Table](../../../PowerBI/writeback-table/overview.md) enables users to edit data directly within Power BI and save the changes to a database, file or service.  

To create a Flow that saves the data from the Writeback Table, add this trigger. The trigger starts the Flow when the user presses the `Save` button in the [Writeback Table](../../../PowerBI/writeback-table/overview.md), and outputs a [DeltaSet](../../api-reference/built-in-types/deltaset.md) containing the changes made by the user. You then need to add an action for the specific target system that you want to save the data to, for example [SQL Server / Azure SQL](../../actions/sql-server/save-deltaset.md), PostgreSQL or [Snowflake](../../actions/snowflake/save-deltaset.md).

<br/>

![Powerbi Writeback Table Trigger](/images/flow/powerbi-writeback-table-trigger.png)

**Example**![Example](/images/strz.jpg)  
This flow is triggered when data is written back from a Power BI Writeback Table. The trigger receives a delta set containing the updated values, which are then saved into the target database table. In this example, the flow captures sales forecast data from Power BI and persists it in the database for further analysis and reporting.

<br/>

## Properties
| Name            | Type            | Description                               |
|-----------------|-----------------|-------------------------------------------|
| Test data       | No              | Use this property to define test data to save to a database, file or service. The test data is only used when you run the Flow manually from the Designer during development. The test data is NOT used when data is saved from the Power BI user interface. [See below](#test-data-format) for a description of the required format. | 

<br/>

## Returns
The trigger returns a [DeltaSet](../../api-reference/built-in-types/deltaset.md) containing all user-made changes intended for storage in a database, file, or service.  
The DeltaSet can be passed to an action that saves the changes to a database, file or service.  

The following actions currently supports saving DeltaSet changes:  
- **SQL Server / Azure SQL**: [Save DeltaSet to SQL Server](../../actions/sql-server/save-deltaset.md)  
- **Snowflake**: [Save DeltaSet to Snowflake](../../actions/snowflake/save-deltaset.md)

<br/>

### Test data format
This describes the format you can use to test saving changes from the Writeback Table before you have added it to a page in Power BI.
To manually run a Flow for testing purposes, press the `Run` button in the Flow Designer application toolbar.  

```json
{
    "updates": [
        {
            "column": "columnName",
            "value": "string | number | boolean",
            "context": {"column1": "value1", "column2": "value2"}
        }
    ]
}
```

#### Example - Test data
The example below shows a test data JSON object that describes changes to January and February amounts of how much a sales representative is expected to sell for a specific product.
```json
{
    "updates": [
        {
            "column": "Jan",
            "value": 3500.59,
            "context": {"ProductId": "100", "SalesRepId": "56-08-12"}
        },
        {
            "column": "Feb",
            "value": 7800,
            "context": {"ProductId": "100", "SalesRepId": "56-08-12"}
        }
    ]
}
```