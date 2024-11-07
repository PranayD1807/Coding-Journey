# Transaction

- A transaction is a unit of work performed in a logical sequence against a database.
- The sequence of operations is crucial within a transaction.
- A transaction contains one or more SQL statements that either execute successfully as a whole or fail.
- Transactions are atomic in nature:
  - If the transaction succeeds: The entire sequence of operations is completed.
  - If the transaction fails: Any changes performed are rolled back, meaning none of the operations in the sequence are applied.

## ACID Properties

- To ensure data integrity, the database system must maintain the following properties of a transaction:

1. **Atomicity**: All operations in a transaction are either fully reflected in the database or not reflected at all.
2. **Consistency**:
   - Integrity constraints must be maintained both before and after the transaction.
   - The database must remain consistent.
3. **Isolation**:
   - Multiple transactions can occur concurrently without interfering with each other.
   - The database ensures that, for every pair of transactions \( T_i \) and \( T_j \), it appears to \( T_i \) that either \( T_j \) finished execution before \( T_i \) started, or \( T_j \) started execution after \( T_i \) finished. Thus, each transaction operates as if it were unaware of other concurrently executing transactions.
4. **Durability**: Once a transaction completes successfully, its changes to the database persist, even in the event of system failures.

## Transaction States

![Transaction States Diagram](https://media.geeksforgeeks.org/wp-content/uploads/20200501195048/Tt7.png)

1. **Active State**:

   - All read and write operations are performed in this state. If all operations are completed successfully, the state changes to the Partially Committed state. If a failure occurs during any read/write operation, the transaction transitions to the Failed state.

2. **Partially Committed State**:

   - After all read/write operations are completed and the transaction is executed, changes are stored in a local buffer in main memory. If these changes are then permanently written to the database, the state changes to Committed; otherwise, it transitions to the Failed state.

3. **Committed State**:

   - At this point, a new consistent state is achieved.
   - Rollback cannot be performed once in the Committed state.
   - This state indicates that changes have been permanently applied to the database.

4. **Failed State**:

   - If a failure occurs while the transaction is executing, making it impossible to continue, the transaction enters the Failed state.

5. **Aborted State**:

   - When a transaction reaches the Failed state, all changes made in the buffer are reversed, and the transaction is fully rolled back. Once this process is complete, the transaction reaches the Aborted state, and the database returns to its prior state before the transaction began.

6. **Terminated State**:
   - A transaction is considered terminated once it has either committed or aborted.

# Useful Links

- [Transaction States in DBMS](https://www.geeksforgeeks.org/transaction-states-in-dbms/)
- [Transaction in DBMS](https://www.geeksforgeeks.org/transaction-in-dbms/)
- [ACID Properties & Transactions in DBMS - YouTube](https://youtu.be/sS4gadQw5iM?si=tZjMEur2UFsZ_uIk)
