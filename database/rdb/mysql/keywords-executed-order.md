# 查询语句不同元素（where、jion、limit、group by、having等等）执行先后顺序？

1.查询中用到的关键词主要包含六个，并且他们的顺序依次为 `select--from--where--group by--having--order by`

其中select和from是必须的，其他关键词是可选的，这六个关键词的执行顺序 与sql语句的书写顺序并不是一样的，而是按照下面的顺序来执行

* from:需要从哪个数据表检索数据
* where:过滤表中数据的条件
* group by:如何将上面过滤出的数据分组
* having:对上面已经分组的数据进行过滤的条件
* select:查看结果集中的哪个列，或列的计算结果
* order by :按照什么样的顺序来查看返回的数据

2.from后面的表关联，是自右向左解析 而where条件的解析顺序是自下而上的。

也就是说，在写SQL文的时候，尽量把数据量小的表放在最右边来进行关联（用小表去匹配大表），而把能筛选出小量数据的条件放在where语句的最左边 （用小表去匹配大表）



# 什么是临时表，临时表什么时候删除？

时表可以手动删除：

```
DROP
TEMPORARY
TABLE
IF
EXISTS
 temp_tb;

```

**临时表只在当前连接可见，当关闭连接时，MySQL会自动删除表并释放所有空间。因此在不同的连接中可以创建同名的临时表，并且操作属于本连接的临时表**。

创建临时表的语法与创建表语法类似，不同之处是**增加关键字TEMPORARY**，如：

```
CREATE TEMPORARY TABLE tmp_table (
	NAME VARCHAR (10) NOT NULL,
	time date NOT NULL
);

select * from tmp_table;
```



