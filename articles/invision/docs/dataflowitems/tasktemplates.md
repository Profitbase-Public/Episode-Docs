
# Tasks Templates

Task Templates contain Tasks being executed by Data Flow Items.
<br/>

![pic](https://profitbasedocs.blob.core.windows.net/images/DataFlowItemTasks%20(1).png)
<br/>
InVision has a number of built-in Tasks that can be used out of the box by adding them to the Data Flow Item under the Task folder.
<br/>
<br/>

![pic](https://profitbasedocs.blob.core.windows.net/images/DataFlowItemTasks%20(2).png)


## Generic SQL Tasks
<br/>

### Generic SQL Script

Executes a T-SQL script which does not return a value.  
**Return value** : None
<br/>


### Create Random Object Name

Generates a random name that can be used as a Table or View name.  
**Return value** : Database Object Name : String
<br/>


### Create Database Table

Create a database table from the schema configuration and returns the name of the table. If the name is not specified, a random name will be used.  
**Return value** : The name of the table that was created

<br/>

### Copy Database Table

Creates a table based on the schema of the souce table and returns the name of the table that was created. En kan spesifisere om innhold skal kopieres i tillegg til skjema.  
If the name of the target table is not specified, or is set to **Auto**, a random name will be created.  
**Return value** : The name of the table that was created

<br/>

### Update Database Table

Updates a target table by merging in new and updates values from a source table. No rows will be deleted from the target table.  
**Return value** : None
<br/>

### Drop Database Table

Drops a table from the database.  
**Return value** : The name of the table that was dropped
<br/>

### Change Data Capture – Is Enabled for Database

Checks if Change Data Capture is enabled for the Database.  
**Return value** : Boolean (true / false)
<br/>

### Change Data Capture – Enable for Database

Enables Change Data Capture for the database and returns a value indicating wether the operation was successful.  
**Return value** : Boolean (true/false)
<br/>

### Change Data Capture – Disable for Database

Disables Change Data Capture for the database and returns a value indicating wether the operation was successful.  
**Return value** : Boolean (true/false)
<br/>

### Change Data Capture – Is Enabled for Table

Checks if Change Data Capture is enabled for the database table.  
**Return value** : Boolean (true/false)
<br/>

#### Change Data Capture – Enable for Table

Enables Change Data Capture for a database table and returns a value indicating wether the operation was successful.  
**Return value** : Boolean(true/false)
<br/>

#### Change Data Capture – Disable for Table

Disables Change Data Capture for a database table and returns a value indicating wether the operation was successful.  
**Return value** : Boolean(true/false)
<br/>

### Workflow Tasks
<br/>

#### Start New Iteration

Starts a new iteration of the specified Workflow. If the startup activity is an [Action Activity](../workflow/processflow/activity.md) set up for automatic execution, the activity will run and execute any attached Data Flows. Subsequent Action Activities set up for automatic execution will run in turn.
<br/>

#### Archive Data Set Members

Archives all members of the Workflow [dataset](../workflow/dataset.md). The archived transactions are tagged with the version id of the curent workflow iteration.
<br/>

#### Set Flow Control Object State

Sets the [state](../workflow/processflow/flowcontrolconfig.md) for a [Flow Control Object](../workflow/flowcontrolobj.md) for a specific Data Context.

**Parameters:**

``Workflow Id``  
>The Workflow Id, can be obtained by right clicking the Workflow in the Solution Explorer and choosing "Copy Id to Clipboard".

``Flow Control Object Id``  
>The Id of the Flow Control Objektet. The Id is available in the Flow Control Objekt editor.

``Data Context``  
>The Data Context is the mechanism that defines the transaction set being processed by a Data Flow, for example a Financial Simulation, Distributor or Spawner.

``StateId``  
>The Id of the state to set. The Id is available in the [Flow Control Object Setup](../workflow/processflow/flowcontrolconfig.md) editor. 


<br/>


### Rollover Tasks
![pic](https://profitbasedocs.blob.core.windows.net/images/DSIt.png)
<br/>


#### Resolve Rollover Delta
<br/>

**Parameters:**

``rolloverPeriodsTableName``

``connectionString`` 

``rolloverFromKeyValue``

``rolloverToKeyValue``

``keyColumnName``  
Optional

``periodNumColumnName``  
Optional

<br/>


#### Store Group/Run Full Rollover

Runs the rollover for all Rollover enabled Data Stores and Settings that are descendants of the Store Group. The actions executed for each Data Store and Setting are determined based on Rollover configuration for each Data Store and Setting. 
<br/>

**Parameters:**

``StoreGroupId``  
>The metadata id of the Store Group. You can use the @Object[…].Id directive as argument to the @[StoreGroupId] parameter to specify the id.

``Rollover delta``  
>The number of Time Frame periods to roll forward.

``Update Independent Stores``  
>Specifies whether to roll forward descendant Data Stores that does not inherit the Time Frame configuration of the Store Group.

<br/>


#### Store Group/Set Time Frame Store Reference Date

Sets the Store Reference Date of the Time Frame for Store Group.
<br/>

**Parameters:**

``StoreGroupId``  
>The metadata id of the Store Group. You can use the @Object[…].Id directive as argument to the @[StoreGroupId] parameter to specify the id.

``newDate``  
>The date to set as the new Store Reference Date. Expects a value of type System.DateTime. 

<br/>


#### Store Group/Set Time Frame Source Reference Date

Sets the Source Reference Date of the Time Frame for Store Group.

<br/>

**Parameters:**

``StoreGroupId``  
The metadata id of the Store Group. You can use the @Object[…].Id directive as argument to the @[StoreGroupId] parameter to specify the id.

``newDate``  
The date to set as the new Source Reference Date. Expects a value of type System.DateTime.

<br/>


#### Store Group/Rollover Time Frame

Calculates the new Store and Source Reference Dates of the Time Frame and updates the values.
<br/>

**Parameters:**

``StoreGroupId``  
>The metadata id of the Store Group. You can use the @Object[…].Id directive as argument to the @[StoreGroupId] parameter to specify the id.

``rolloverDelta``  
>The number of Time Frame periods to roll forward.