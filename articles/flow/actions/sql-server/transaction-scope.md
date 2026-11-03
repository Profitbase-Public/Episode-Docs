# Transaction Scope

Sets a transaction scope that ensures all following SQL operations either succeed or fail together.


![Transaction Scope](https://profitbasedocs.blob.core.windows.net/flowimages/transaction-scope.png)


## Properties

| Name             | Required | Description                                       |
|------------------|----------|---------------------------------------------------|
| Title            | No | The descriptive title of the action.              |
| Timeout (sec)    | No | The transaction scope timeout in seconds.          |
| Isolation level  | No | Specifies the transaction isolation level. The default is `Serializable`.  |
| Description      | No | Additional notes or comments about the action or configuration. |

### Isolation levels

| Level            |  Description                                       |
|------------------|------------------------------------------------------|
| Read Uncommitted | Specifies that statements can read rows that were modified by other transactions but not yet committed. |
| Read Committed   | Specifies that statements can't read data that was modified but not committed by other transactions. This prevents dirty reads. |
| Repeatable Read  | Specifies that statements can't read data that was modified but not yet committed by other transactions, and that no other transactions can modify data that was read by the current transaction until the current transaction completes. |
| Snapshot | Specifies that data read by any statement in a transaction is the transactionally consistent version of the data that existed at the start of the transaction. |
| Serializable | Statements can't read data that was modified but not yet committed by other transactions. No other transactions can modify data that was read by the current transaction until the current transaction completes. Other transactions can't insert new rows with key values that would fall in the range of keys read by any statements in the current transaction until the current transaction completes. |

For details, see the [Microsoft documentation](https://learn.microsoft.com/en-us/sql/t-sql/statements/set-transaction-isolation-level-transact-sql?view=sql-server-ver17#arguments).


<br/>

[!INCLUDE [](__videos.md)]