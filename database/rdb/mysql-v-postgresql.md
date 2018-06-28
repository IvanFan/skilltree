Here’s a refresher:

![](https://cdn-images-1.medium.com/max/1600/1*LtiA09xS90-5qbLdHquGiA.png)

#### Process vs Thread {#fe45}

As[Postgres forks off a child process](https://www.postgresql.org/docs/10/static/tutorial-arch.html)to establish a connection, it can take[up to 10 MB per connection](https://www.citusdata.com/blog/2017/05/10/scaling-connections-in-postgres/). The memory pressure is bigger compared to MySQL’s thread-per-connection model, where the default[stack size of a thread](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_thread_stack)is at 256KB on 64-bit platforms. \(Of course, thread-local sort buffers, etc. make this overhead less significant, if not negligible, but still.\)

Even though[copy-on-write](https://en.wikipedia.org/wiki/Copy-on-write)saves some of the shared, immutable memory state with the parent process, the basic overhead of being a process-based architecture is taxing when you have 1,000+ concurrent connections, and it can be one of the most important factors for capacity planning.

#### Clustered Index vs Heap Table {#a0ec}

A[clustered index](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-2017)is a table structure where rows are directly embedded inside the B-tree structure of its primary key. A \(non-clustered\) heap is a regular table structure filled with data rows separately from indexes.

With a clustered index, when you look up a record by the primary key, a single I/O will retrieve the entire row, whereas non-clustered always require at least two I/Os by following the reference. The impact can be significant as foreign key reference and joins will trigger primary key lookup, which account for vast majority of queries.

A theoretical downside of clustered index is that it requires twice as many tree node traversal when you query with a secondary index, as you first scan over the secondary index, then walk through the clustered index, which is also a tree.

But given a modern[convention](https://en.wikipedia.org/wiki/Convention_over_configuration)to have an auto-increment integer as a primary key¹ — it’s called a[surrogate key](https://en.wikipedia.org/wiki/Surrogate_key) — it is[almost always desirable to have a clustered index](https://www.mssqltips.com/sqlservertutorial/3209/make-sure-all-tables-have-a-clustered-index-defined/). It is more so if you do a lot of`ORDER BY id`to retrieve the most recent \(or oldest\) N records, which I believe applies to most.

Postgres does not support clustered index, whereas MySQL \(InnoDB\) does not support heap. But either way, the difference should be minor if you have a large amount of memory.

