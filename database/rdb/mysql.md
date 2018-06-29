## Transaction Isolation level

Transaction isolation is one of the foundations of database processing. Isolation is the I in the acronym [ACID](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_acid); the isolation level is the setting that fine-tunes the balance between performance and reliability, consistency, and reproducibility of results when multiple transactions are making changes and performing queries at the same time.

```
InnoDB offers all four transaction isolation levels described by the SQL:1992 standard: 
READ UNCOMMITTED, 
READ COMMITTED, 
REPEATABLE READ, 
and SERIALIZABLE. The default isolation level for InnoDB is REPEATABLE READ.
```



