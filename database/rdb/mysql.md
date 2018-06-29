## Transaction Isolation level

Transaction isolation is one of the foundations of database processing. Isolation is the I in the acronym [ACID](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_acid); the isolation level is the setting that fine-tunes the balance between performance and reliability, consistency, and reproducibility of results when multiple transactions are making changes and performing queries at the same time.

```
InnoDB offers all four transaction isolation levels described by the SQL:1992 standard: 
READ UNCOMMITTED, 
READ COMMITTED, 
REPEATABLE READ, 
and SERIALIZABLE. The default isolation level for InnoDB is REPEATABLE READ.
```

```
A user can change the isolation level for a single session or for all subsequent connections with the
 SET TRANSACTION statement. 
 To set the server's default isolation level for all connections, 
 use the --transaction-isolation option on the command line or in an option file.
```

The following list describes how MySQL supports the different transaction levels. The list goes from the most commonly used level to the least used.

### REPEATABLE READ

### 

### ![](/assets/Screen Shot 2018-06-29 at 3.22.10 pm.png)READ COMMITTED

### ![](/assets/Screen Shot 2018-06-29 at 3.25.25 pm.png)READ UNCOMMITTED

[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)statements are performed in a nonlocking fashion, but a possible earlier version of a row might be used. Thus, using this isolation level, such reads are not consistent. This is also called a [dirty read](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_dirty_read). Otherwise, this isolation level works like[`READ COMMITTED`](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html#isolevel_read-committed).

### SERIALIZABLE

![](/assets/Screen Shot 2018-06-29 at 3.27.56 pm.png)事务隔离级别

|  | 脏读 | 不可重复读 | 幻读 |
| :--- | :--- | :--- | :--- |
| 读未提交 read-uncommitted | 是 | 是 | 是 |
| 不可重复读 read-committed | 否 | 是 | 是 |
| 可重复读 repeatable-read | 否 | 否 | 是 |
| 串行化 serializable | 否 | 否 | 否 |

* **读未提交**
  ：另一个事务修改了数据，但尚未提交，而本事务中的SELECT会读到这些未被提交的数据
  **脏读**
* **不可重复读**
  ：事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果因此本事务先后两次读到的数据结果会不一致。
* **可重复读**
  ：在同一个事务里，SELECT的结果是事务开始时时间点的状态，因此，同样的SELECT操作读到的结果会是一致的。但是，会有
  **幻读**
  现象
* **串行化**
  ：最高的隔离级别，在这个隔离级别下，不会产生任何异常。并发的事务，就像事务是在一个个按照顺序执行一样

SQL规范所规定的标准，不同的数据库具体的实现可能会有些差异

1. MySQL中默认事务隔离级别是“可重复读”时并不会锁住读取到的行

2. **事务隔离级别**  
   ：  
   **未提交读时**  
   ，写数据只会锁住相应的行。

3. **事务隔离级别为**
   ：
   **可重复读时**
   ，写数据会锁住整张表。
4. **事务隔离级别为**
   ：
   **串行化时**
   ，读写数据都会锁住整张表。

**隔离级别越高**，**越能保证数据的完整性和一致性**，但是对并发性能的影响也越大，鱼和熊掌不可兼得啊。**对于多数应用程序，可以优先考虑把数据库系统的隔离级别设为Read Committed，它能够避免脏读取，而且具有较好的并发性能**。尽管它会导致不可重复读、幻读这些并发问题，在可能出现这类问题的个别场合，可以由应用程序采用悲观锁或乐观锁来控制





MySQL 3 search engines: InnoDB, MyISAM, MEMORY

