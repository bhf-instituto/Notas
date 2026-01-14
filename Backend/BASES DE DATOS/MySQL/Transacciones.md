MySQL transactions are a sequence of SQL statements treated as a single, indivisible unit of work. They ensure that either all operations within the group succeed, or none of them are applied, which is crucial for maintaining data integrity and consistency. 

## The ACID Properties

Transactions in MySQL (specifically with the `InnoDB` storage engine, the default) adhere to the **ACID** properties: 

- **Atomicity:** Guarantees that the transaction is an "all-or-nothing" unit. If any part fails, the entire transaction is rolled back to its original state.
- **Consistency:** Ensures that a transaction brings the database from one valid state to another, enforcing all defined rules and constraints (like foreign keys or unique keys).
- **Isolation:** Transactions operate independently of each other. Changes made by one transaction are not visible to others until they are committed, preventing interference between concurrent operations.
- **Durability:** Once a transaction is committed, the changes are permanent and will survive any subsequent system failures, such as a power outage or crash. 

### Key Transaction Commands

You control MySQL transactions using specific SQL statements: 

| **Command**           | **Description**                                                                                                                                                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `START TRANSACTION`   | Marks the beginning of a new transaction. `BEGIN` is an alias for this command.                                                                                                                                                  |
| `COMMIT`              | Permanently saves all changes made during the current transaction to the database.                                                                                                                                               |
| `ROLLBACK`            | Undoes all changes made during the current transaction, reverting the database to the state it was in before the transaction started.                                                                                            |
| `SAVEPOINT`           | Creates a named checkpoint within a transaction, allowing for partial rollbacks to that specific point using `ROLLBACK TO SAVEPOINT`.                                                                                            |
| `SET autocommit = 0;` | By default, MySQL operates in `autocommit` mode, where every single SQL statement is its own transaction. Setting `autocommit` to `0` disables this for the current session, requiring explicit `COMMIT` or `ROLLBACK` commands. |
### Example: Transferring Funds

A common example is a bank transfer. To ensure data integrity, both the debit and the credit must happen together:
```mysql
START TRANSACTION;

-- Deduct amount from sender's account
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- Add amount to receiver's account
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

-- If both operations succeed, commit the changes
COMMIT;
```