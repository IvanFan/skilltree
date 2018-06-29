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



