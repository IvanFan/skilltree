Concurrency Control deals with **interleaved execution **of more than one transaction

**What is Transaction?**

A set of logically related operations is known as transaction. The main operations of a transaction are:

**Read\(A\):**Read operations Read\(A\) or R\(A\) reads the value of A from the database and stores it in a buffer in main memory.

**Write \(A\):**Write operation Write\(A\) or W\(A\) writes the value back to the database from buffer.

But it may also be possible that transaction may fail after executing some of its operations. The failure can be because of**hardware, software or power**etc. For example, if debit transaction discussed above fails after executing operation 2, the value of A will remain 5000 in the database which is not acceptable by the bank. To avoid this, Database has two important operations:

**Commit:**After all instructions of a transaction are successfully executed, the changes made by transaction are made permanent in the database.

**Rollback:**If a transaction is not able to execute all operations successfully, all the changes made by transaction are undone.

## **Properties of a transaction**

**Atomicity:**As a transaction is set of logically related operations,**either all of them should be executed or none**. A debit transaction discussed above should either execute all three operations or none.If debit transaction fails after executing operation 1 and 2 then its new value 4000 will not be updated in the database which leads to inconsistency.

**Consistency:**If operations of debit and credit transactions on same account are executed concurrently, it may leave database in an inconsistent state.

* For Example, T1 \(debit of Rs. 1000 from A\) and T2 \(credit of 500 to A\) executing concurrently, the database reaches inconsistent state.
* Let us assume Account balance of A is Rs. 5000. T1 reads A\(5000\) and stores the value in its local buffer space. Then T2 reads A\(5000\) and also stores the value in its local buffer space.
* T1 performs A=A-1000 \(5000-1000=4000\) and 4000 is stored in T1 buffer space. Then T2 performs A=A+500 \(5000+500=5500\) and 5500 is stored in T2 buffer space. T1 writes the value from its buffer back to database.
* A’s value is updated to 4000 in database and then T2 writes the value from its buffer back to database. A’s value is updated to 5500 which shows that the effect of debit transaction is lost and database has become inconsistent.
* To maintain consistency of database, we need
  **concurrency control protocols**
  which will be discussed in next article.  The operations of T1 and T2 with their buffers and database have been shown in Table 1.

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

**Isolation:**Result of a transaction should not be visible to others before transaction is committed. For example, Let us assume that A’s balance is Rs. 5000 and T1 debits Rs. 1000 from A. A’s new balance will be 4000. If T2 credits Rs. 500 to A’s new balance, A will become 4500 and after this T1 fails. Then we have to rollback T2 as well because it is using value produced by T1. So a transaction results are not made visible to other transactions before it commits.

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
  When operations of a transaction are interleaved with operations of other transactions of a schedule, the schedule is called Concurrent schedule. e.g.; Schedule of debit and credit transaction shown in Table 1 is concurrent in nature. But concurrency can lead to inconsistency in database.  The above example of concurrent schedule is also inconsistent.

## 

### Why is concurrency control needed?\[[edit](https://en.wikipedia.org/w/index.php?title=Concurrency_control&action=edit&section=3)\]

If transactions are executedserially, i.e., sequentially with no overlap in time, no transaction concurrency exists. However, if concurrent transactions with interleaving operations are allowed in an uncontrolled manner, some unexpected, undesirable results may occur, such as:

1. **The lost update problem**: A second transaction writes a second value of a data-item \(datum\) on top of a first value written by a first concurrent transaction, and the first value is lost to other transactions running concurrently which need, by their precedence, to read the first value. The transactions that have read the wrong value end with incorrect results.
2. **The dirty read problem**: Transactions read a value written by a transaction that has been later aborted. This value disappears from the database upon abort, and should not have been read by any transaction \("dirty read"\). The reading transactions end with incorrect results.
3. **The incorrect summary problem**: While one transaction takes a summary over the values of all the instances of a repeated data-item, a second transaction updates some instances of that data-item. The resulting summary does not reflect a correct result for any \(usually needed for correctness\) precedence order between the two transactions \(if one is executed before the other\), but rather some random result, depending on the timing of the updates, and whether certain update results have been included in the summary or not.

Most high-performance transactional systems need to run transactions concurrently to meet their performance requirements. Thus, without concurrency control such systems can neither provide correct results nor maintain their databases consistently.

### Concurrency control mechanisms\[[edit](https://en.wikipedia.org/w/index.php?title=Concurrency_control&action=edit&section=4)\]

#### Categories\[[edit](https://en.wikipedia.org/w/index.php?title=Concurrency_control&action=edit&section=5)\]

The main categories of concurrency control mechanisms are:

* [**Optimistic**](https://en.wikipedia.org/wiki/Optimistic_concurrency_control)
  - Delay the checking of whether a transaction meets the isolation and other integrity rules \(e.g.,
  [serializability](https://en.wikipedia.org/wiki/Serializability)
  and
  [recoverability](https://en.wikipedia.org/wiki/Serializability#Correctness_-_recoverability)
  \) until its end, without blocking any of its \(read, write\) operations \("...and be optimistic about the rules being met..."\), and then abort a transaction to prevent the violation, if the desired rules are to be violated upon its commit. An aborted transaction is immediately restarted and re-executed, which incurs an obvious overhead \(versus executing it to the end only once\). If not too many transactions are aborted, then being optimistic is usually a good strategy.
* **Pessimistic**
  - Block an operation of a transaction, if it may cause violation of the rules, until the possibility of violation disappears. Blocking operations is typically involved with performance reduction.
* **Semi-optimistic**
  - Block operations in some situations, if they may cause violation of some rules, and do not block in other situations while delaying rules checking \(if needed\) to transaction's end, as done with optimistic.

Different categories provide different performance, i.e., different average transaction completion rates \(throughput\), depending on transaction types mix, computing level of parallelism, and other factors. If selection and knowledge about trade-offs are available, then category and method should be chosen to provide the highest performance.

The mutual blocking between two transactions \(where each one blocks the other\) or more results in a[deadlock](https://en.wikipedia.org/wiki/Deadlock), where the transactions involved are stalled and cannot reach completion. Most non-optimistic mechanisms \(with blocking\) are prone to deadlocks which are resolved by an intentional abort of a stalled transaction \(which releases the other transactions in that deadlock\), and its immediate restart and re-execution. The likelihood of a deadlock is typically low.

Blocking, deadlocks, and aborts all result in performance reduction, and hence the trade-offs between the categories.

#### Methods\[[edit](https://en.wikipedia.org/w/index.php?title=Concurrency_control&action=edit&section=6)\]

Many methods for concurrency control exist. Most of them can be implemented within either main category above. The major methods,[\[1\]](https://en.wikipedia.org/wiki/Concurrency_control#cite_note-Bern2009-1)which have each many variants, and in some cases may overlap or be combined, are:

1. Locking \(e.g.,
   [**Two-phase locking**](https://en.wikipedia.org/wiki/Two-phase_locking)
   - 2PL\) - Controlling access to data by
   [locks](https://en.wikipedia.org/wiki/Lock_%28computer_science%29)
   assigned to the data. Access of a transaction to a data item \(database object\) locked by another transaction may be blocked \(depending on lock type and access operation type\) until lock release.
2. **Serialization**
   [**graph checking**](https://en.wikipedia.org/wiki/Serializability#Testing_conflict_serializability)
   \(also called Serializability, or Conflict, or Precedence graph checking\) - Checking for
   [cycles](https://en.wikipedia.org/wiki/Cycle_%28graph_theory%29)
   in the schedule's
   [graph](https://en.wikipedia.org/wiki/Directed_graph)
   and breaking them by aborts.
3. [**Timestamp ordering**](https://en.wikipedia.org/wiki/Timestamp-based_concurrency_control)
   \(TO\) - Assigning timestamps to transactions, and controlling or checking access to data by timestamp order.
4. [**Commitment ordering**](https://en.wikipedia.org/wiki/Commitment_ordering)
   \(or Commit ordering; CO\) - Controlling or checking transactions' chronological order of commit events to be compatible with their respective
   [precedence order](https://en.wikipedia.org/wiki/Serializability#Testing_conflict_serializability)
   .

Other major concurrency control types that are utilized in conjunction with the methods above include:

* [**Multiversion concurrency control**](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)
  \(MVCC\) - Increasing concurrency and performance by generating a new version of a database object each time the object is written, and allowing transactions' read operations of several last relevant versions \(of each object\) depending on scheduling method.
* [**Index concurrency control**](https://en.wikipedia.org/wiki/Index_locking)
  - Synchronizing access operations to
  [indexes](https://en.wikipedia.org/wiki/Index_%28database%29)
  , rather than to user data. Specialized methods provide substantial performance gains.
* **Private workspace model**
  \(
  **Deferred update**
  \) - Each transaction maintains a private workspace for its accessed data, and its changed data become visible outside the transaction only upon its commit \(e.g.,
  [Weikum and Vossen 2001](https://en.wikipedia.org/wiki/Concurrency_control#Weikum01)
  \). This model provides a different concurrency control behavior with benefits in many cases.

The most common mechanism type in database systems since their early days in the 1970s has been[Strong strict Two-phase locking](https://en.wikipedia.org/wiki/Two-phase_locking)\(SS2PL; also calledRigorous schedulingorRigorous 2PL\) which is a special case \(variant\) of both[Two-phase locking](https://en.wikipedia.org/wiki/Two-phase_locking)\(2PL\) and[Commitment ordering](https://en.wikipedia.org/wiki/Commitment_ordering)\(CO\). It is pessimistic. In spite of its long name \(for historical reasons\) the idea of the**SS2PL**mechanism is simple: "Release all locks applied by a transaction only after the transaction has ended." SS2PL \(or Rigorousness\) is also the name of the set of all schedules that can be generated by this mechanism, i.e., these are SS2PL \(or Rigorous\) schedules, have the SS2PL \(or Rigorousness\) property.

  


## 

## Implementation of Locking in DBMS

## 

Locking protocols are used in database management systems as a means of concurrency control. Multiple transactions may request a lock on a data item simultaneously.

Hence, we require a mechanism to manage the locking requests made by transactions. Such a mechanism is called as

**Lock Manager**

. It relies on the process of message passing where transactions and lock manager exchange messages to handle the locking and unlocking of data items.

**Data structure used in Lock Manager –**  
The data structure required for implementation of locking is called as**Lock table**.

1. It is a hash table where name of data items are used as hashing index.
2. Each locked data item has a linked list associated with it.
3. Every node in the linked list represents the transaction which requested for lock, mode of lock requested \(mutual/exclusive\) and current status of the request \(granted/waiting\).
4. Every new lock request for the data item will be added in the end of linked list as a new node.
5. Collisions in hash table are handled by technique of separate chaining.

Consider the following example of lock table:

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Slide1-4.jpg)

**Explanation:**In the above figure, the locked data items present in lock table are 5, 47, 167 and 15.

The transactions which have requested for lock have been represented by a linked list shown below them using a downward arrow.

Each node in linked list has the name of transaction which has requested the data item like T33, T1, T27 etc.

The colour of node represents the status i.e. whether lock has been granted or waiting.

Note that a collision has occurred for data item 5 and 47. It has been resolved by separate chaining where each data item belongs to a linked list. The data item is acting as header for linked list containing the locking request.

**Working of Lock Manager**

1. Initially the lock table is table empty as no data item is locked.
2. Whenever lock manger receives a lock request from a transaction Ti on a particular data item Qi following cases may arise

```
If Qi is not already locked, a linked list will be created and lock will be granted to the requesting transaction Ti.
If the data item is already locked, a new node will be added at the end of its linked list containing the information about erquest made by Ti.
```

```
3.  If the lock mode requested by Ti is compatible with lock mode of transaction currently having the lock, Ti will acquire the lock too and status will be changed to ‘granted’. Else, status of Ti’s lock will be ‘waiting’
```

```
4. If a transaction Ti wants to unlock the data item it is currently holding, it will send an unlock request to the lock manager. The lock manger will delete Ti’s node from this linked list. Lock will be granted to the next transaction in the list.
```

```
5.Sometimes transaction Ti may have to be aborted. In such a case all the waiting request made by Ti will be deleted from the linked lists present in lock table. Once abortion is complete, locks held by Ti will also be released.
```

## serial schedules

serial schedules have less resource utilization and low throughput. To improve it, two are more transactions are run concurrently. But concurrency of transactions may lead to inconsistency in database. To avoid this, we need to check whether these concurrent schedules are serializable or not.

**onflict Serializable:**A schedule is called conflict serializable if it can be transformed into a serial schedule by swapping non-conflicting operations.

**Conflicting operations:**Two operations are said to be conflicting if all conditions satisfy:

* They belong to different transaction
* They operation on same data item
* At Least one of them is a write operation

```
Example: –

Conflicting operations pair (R1(A), W2(A)) because they belong to two different transactions on same data item A and one of them is write operation.
Similarly, (W1(A), W2(A)) and (W1(A), R2(A)) pairs are also conflicting.
On the other hand, (R1(A), W2(B)) pair is non-conflicting because they operate on different data item.
Similarly, ((W1(A), W2(B)) pair is non-conflicting.
```

**Conflict Equivalent:**

Two schedules are said to be conflict equivalent when one can be transformed to another by swapping non-conflicting operations. In the example discussed above, S11 is conflict equivalent to S1 \(S1 can be converted to S11 by swapping non-conflicting operations\). Similarly, S11 is conflict equivalent to S12 and so on.

## Recoverability of Schedules

As discussed, a transaction may not execute completely due to hardware failure, system crash or software issues. In that case, we have to rollback the failed transaction. But some other transaction may also have used values produced by failed transaction. So we have to rollback those transactions as well.

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/scheduleDBMS.png "Recoverabilityofschedules")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/scheduleDBMS.png)  
Above table shows a schedule with two transactions, T1 reads and writes A and that value is read and written by T2. T2 commits. But later on, T1 fails. So we have to rollback T1. Since T2 has read the value written by T1, it should also be rollbacked. But we have already committed that. So this schedule is irrecoverable schedule.

**Irrecoverable Schedule:**When Tj is reading the value updated by Ti and Tj is committed before commit of Ti, the schedule will be irrecoverable.

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/schedule5.png "Recoverabilityofschedules2")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/schedule5.png)

Table 2 shows a schedule with two transactions, T1 reads and writes A and that value is read and written by T2. But later on, T1 fails. So we have to rollback T1. Since T2 has read the value written by T1, it should also be rollbacked. As it has not committed, we can rollback T2 as well. So it is recoverable with **cascading rollback.**  
**Recoverable with cascading rollback**: If Tj is reading value updated by Ti and commit of Tj is delayed till commit of Ti , the schedule is called recoverable with cascading rollback.

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/schedult3.png "Recoverability3")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/schedult3.png)

Table 3 shows a schedule with two transactions, T1 reads and writes A and commits and that value is read by T2. But if T1 fails before commit, no other transaction has read its value, so there is no need to rollback other transaction. So this is a **cascadeless recoverable schedule.**

**Cascadeless Recoverable:**

If Tj reads value updated by Ti only after Ti is commited, the schedule will be cascadeless recoverable.

Now, we all know the four properties a transaction must follow. Yes, you got that right, I mean the[**ACID**properties](https://www.geeksforgeeks.org/acid-properties-in-dbms/). Concurrency control techniques are used to ensure that the_Isolation_\(or non-interference\) property of concurrently executing transactions is maintained.

_A trivial question I would like to pose in front of you, \(I know you must know this but still\) why do you think that we should have interleaving execution of transactions if it may lead to problems such as Irrecoverable Schedule, Inconsistency and many more threats.  
Why not just let it be Serial schedules and we may live peacefully, no complications at all._

**Yes, the performance effects the efficiency too much which is not acceptable.      
**Hence a Database may provide a mechanism that ensures that the schedules are either conflict or view serializable and recoverable \(also preferably cascadeless\). Testing for a schedule for Serializability after it has executed is obviously_too late!_  
So we need Concurrency Control Protocols that ensures Serializability .

**Concurrency-control protocols :**

allow concurrent schedules, but ensure that the schedules are conflict/view serializable, and are recoverable and maybe even cascadeless.

**Lock Based Protocols –**  
A lock is a variable associated with a data item that describes a status of data item with respect to possible operation that can be applied to it. They synchronize the access by concurrent transactions to the database items. It is required in this protocol that all the data items must be accessed in a mutually exclusive manner. Let me introduce you to two common locks which are used and some terminology followed in this protocol.

1. **Shared Lock \(S\):**
   also known as Read-only lock. As the name suggests it can be shared between transactions because while holding this lock the transaction does not have the permission to update data on the data item. S-lock is requested using lock-S instruction.
2. **Exclusive Lock \(X\):**
   Data item can be both read as well as written.This is Exclusive and cannot be held simultaneously on the same data item. X-lock is requested using lock-X instruction.

A transaction may be granted a lock on an item if the requested lock is compatible with locks already held on the item by other transactions.

* Any number of transactions can hold shared locks on an item, but if any transaction holds an exclusive\(X\) on the item no other transaction may hold any lock on the item.
* If a lock cannot be granted, the requesting transaction is made to wait till all incompatible locks held by other transactions have been released. Then the lock is granted. 

```
Upgrade / Downgrade locks : A transaction that holds a lock on an item A is allowed under certain condition to change the lock state from one state to another.
Upgrade: A S(A) can be upgraded to X(A) if Ti is the only transaction holding the S-lock on element A.
Downgrade: We may downgrade X(A) to S(A) when we feel that we no longer want to write on data-item A. As we were holding X-lock on A, we need not check any conditions.
```

```
Deadlock – consider the above execution phase. Now, T1 holds an Exclusive lock over B, and T2 holds a Shared lock over A. Consider Statement 7, T1 requests for lock on B, while in Statement 8 T2 requests lock on A. This as you may notice imposes a Deadlock as none can proceed with their execution.
```



