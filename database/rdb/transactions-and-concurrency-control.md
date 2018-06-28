Concurrency Control deals with **interleaved execution **of more than one transaction

**What is Transaction?**

A set of logically related operations is known as transaction. The main operations of a transaction are:

**Read\(A\):**Read operations Read\(A\) or R\(A\) reads the value of A from the database and stores it in a buffer in main memory.

**Write \(A\):**Write operation Write\(A\) or W\(A\) writes the value back to the database from buffer.



But it may also be possible that transaction may fail after executing some of its operations. The failure can be because of**hardware, software or power**etc. For example, if debit transaction discussed above fails after executing operation 2, the value of A will remain 5000 in the database which is not acceptable by the bank. To avoid this, Database has two important operations:

**Commit:**After all instructions of a transaction are successfully executed, the changes made by transaction are made permanent in the database.

**Rollback:**If a transaction is not able to execute all operations successfully, all the changes made by transaction are undone.



### **Properties of a transaction**

**Atomicity:**As a transaction is set of logically related operations,**either all of them should be executed or none**. A debit transaction discussed above should either execute all three operations or none.If debit transaction fails after executing operation 1 and 2 then its new value 4000 will not be updated in the database which leads to inconsistency.

**Consistency:**If operations of debit and credit transactions on same account are executed concurrently, it may leave database in an inconsistent state.

* For Example, T1 \(debit of Rs. 1000 from A\) and T2 \(credit of 500 to A\) executing concurrently, the database reaches inconsistent state.
* Let us assume Account balance of A is Rs. 5000. T1 reads A\(5000\) and stores the value in its local buffer space. Then T2 reads A\(5000\) and also stores the value in its local buffer space.
* T1 performs A=A-1000 \(5000-1000=4000\) and 4000 is stored in T1 buffer space. Then T2 performs A=A+500 \(5000+500=5500\) and 5500 is stored in T2 buffer space. T1 writes the value from its buffer back to database.
* A’s value is updated to 4000 in database and then T2 writes the value from its buffer back to database. A’s value is updated to 5500 which shows that the effect of debit transaction is lost and database has become inconsistent.
* To maintain consistency of database, we need
  **concurrency control protocols**
  which will be discussed in next article.  The operations of T1 and T2 with their buffers and database have been shown in Table 1.

| **T1** | **T1’s buffer space** | **T2** | **T2’s Buffer Space** | **Database** |
| :--- | :--- | :--- | :--- | :--- |
|  |  |  |  | A=5000 |
| R\(A\); | A=5000 |  |  | A=5000 |
|  | A=5000 | R\(A\); | A=5000 | A=5000 |
| A=A-1000; | A=4000 |  | A=5000 | A=5000 |
|  | A=4000 | A=A+500; | A=5500 |  |
| W\(A\); |  |  | A=5500 | A=4000 |
|  |  | W\(A\); |  | A=5500 |

**Table 1**

**Isolation:**Result of a transaction should not be visible to others before transaction is committed. For example, Let us assume that A’s balance is Rs. 5000 and T1 debits Rs. 1000 from A. A’s new balance will be 4000. If T2 credits Rs. 500 to A’s new balance, A will become 4500 and after this T1 fails. Then we have to rollback T2 as well because it is using value produced by T1. So a transaction results are not made visible to other transactions before it commits.

**Durable:**Once database has committed a transaction, the changes made by the transaction should be permanent. e.g.; If a person has credited $500000 to his account, bank can’t say that the update has been lost. To avoid this problem, multiple copies of database are stored at different locations.

**What is a Schedule?**

A schedule is series of operations from one or more transactions. A schedule can be of two types:

* **Serial Schedule:**
  When one transaction completely executes before starting another transaction, the schedule is called serial schedule. A serial schedule is always consistent. e.g.; If a schedule S has debit transaction T1 and credit transaction T2, possible serial schedules are T1 followed by T2 \(T1-
  &gt;
  T2\) or T2 followed by T1 \(\(T1-
  &gt;
  T2\). A serial schedule has low throughput and less resource utilization.
* **Concurrent Schedule:**
  When operations of a transaction are interleaved with operations of other transactions of a schedule, the schedule is called Concurrent schedule. e.g.; Schedule of debit and credit transaction shown in Table 1 is concurrent in nature. But concurrency can lead to inconsistency in database.  The above example of concurrent schedule is also inconsistent.





