Indexing is a way to optimize performance of a database by minimizing the number of disk accesses required when a query is processed.

An index or database index is a data structure which is used to quickly locate and access the data in a database table.

Indexes are created using some database columns.

* The first column is the Search key that contains a copy of the primary key or candidate key of the table. These values are stored in sorted order so that the corresponding data can be accessed quickly \(Note that the data may or may not be stored in sorted order\).
* The second column is the Data Reference which contains a set of pointers holding the address of the disk block where that particular key value can be found.

[![](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing2.png "indexing2")](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing2.png)

There are two kinds of indices:

1. **Ordered indices:**
   Indices are based on a sorted ordering of the values.
2. **Hash indices:**
   Indices are based on the values being distributed uniformly across a range of buckets. The buckets to which a value is assigned is determined by function called a hash function.

There is no comparison between both the techniques, it depends on the database application on which it is being applied.

* **Access Types**
  : e.g. value based search, range access, etc.
* **Access Time**
  : Time to find particular data element or set of elements.
* **Insertion Time**
  : Time taken to find the appropriate space and insert a new data time.
* **Deletion Time**
  : Time taken to find an item and delete it as well as update the index structure.
* **Space Overhead**
  : Additional space required by the index.

### **Clustered Indexing**

Clustering index is defined on an ordered data file. The data file is ordered on a non-key field. In some cases, the index is created on non-primary key columns which may not be unique for each record. In such cases, in order to identify the records faster, we will group two or more columns together to get the unique values and create index out of them. This method is known as clustering index. Basically, records with similar characteristics are grouped together and indexes are created for these groups.

Data is physically stored with the clustered index

定义：数据行的物理顺序与列值（一般是主键的那一列）的逻辑顺序相同，一个表中只能拥有一个聚集索引。

单单从定义来看是不是显得有点抽象，打个比方，一个表就像是我们以前用的新华字典，聚集索引就像是拼音目录，而每个字存放的页码就是我们的数据物理地址，我们如果要查询一个“哇”字，我们只需要查询“哇”字对应在新华字典拼音目录对应的页码，就可以查询到对应的“哇”字所在的位置，而拼音目录对应的A-Z的字顺序，和新华字典实际存储的字的顺序A-Z也是一样的，如果我们中文新出了一个字，拼音开头第一个是B，那么他插入的时候也要按照拼音目录顺序插入到A字的后面，现在用一个简单的示意图来大概说明一下在数据库中的样子：

| 地址 | id | username | score |
| :--- | :--- | :--- | :--- |
| 0x01 | 1 | 小明 | 90 |
| 0x02 | 2 | 小红 | 80 |
| 0x03 | 3 | 小华 | 92 |
| .. | .. | .. | .. |
| 0xff | 256 | 小英 | 70 |

注：第一列的地址表示该行数据在磁盘中的物理地址，后面三列才是我们SQL里面用的表里的列，其中id是主键，建立了聚集索引。

结合上面的表格就可以理解这句话了吧：数据行的物理顺序与列值的**顺序相同**，如果我们查询id比较靠后的数据，那么这行数据的地址在磁盘中的物理地址也会比较靠后。而且由于物理排列方式与聚集索引的顺序相同，所以也就只能建立一个聚集索引了。

![](https://img.mukewang.com/5a66e4b80001a52b06520488.png "聚集索引示意图")

**聚集索引实际存放的示意图**

从上图可以看出聚集索引的好处了，索引的叶子节点就是对应的数据节点（MySQL的MyISAM除外，此存储引擎的聚集索引和非聚集索引只多了个唯一约束，其他没什么区别），可以直接获取到对应的全部列的数据，而非聚集索引在索引没有覆盖到对应的列的时候需要进行二次查询，后面会详细讲。因此在查询方面，聚集索引的速度往往会更占优势。

### 创建聚集索引

如果不创建索引，系统会自动创建一个隐含列作为表的聚集索引。

1.创建表的时候指定主键（注意：SQL Sever默认主键为聚集索引，也可以指定为非聚集索引，而MySQL里主键就是聚集索引）

```
create table t1
(

    id 
int
 primary key
,

    name nvarchar
(
255
)
)
```

2.创建表后添加聚集索引

**SQL Server**

```
create clustered index clustered_index on table_name
(
colum_name
)
```

**MySQL**

```
alter table table_name add primary key
(
colum_name
)
```

值得注意的是，最好还是在创建表的时候添加聚集索引，由于聚集索引的物理顺序上的特殊性，因此如果再在上面创建索引的时候会根据索引列的排序移动全部数据行上面的顺序，会非常地耗费时间以及性能。

### **Non­-Clustered Indexing**

​A non clustered index just tells us where the data lies, i.e. it gives us a list of virtual pointers or references to the location where the data is actually stored. Data is not physically stored in the order of the index. Instead , data is present in leaf nodes. For eg. the contents page of a book. Each entry gives us the page number or location of the information stored. The actual data here\(information on each page of book\) is not organised but we have an ordered reference\(contents page\) to where the data points actually lie.

定义：该索引中索引的逻辑顺序与磁盘上行的物理存储顺序不同，一个表中可以拥有多个非聚集索引。

其实按照定义，除了聚集索引以外的索引都是非聚集索引，只是人们想细分一下非聚集索引，分成普通索引，唯一索引，全文索引。如果非要把非聚集索引类比成现实生活中的东西，那么非聚集索引就像新华字典的偏旁字典，他结构顺序与实际存放顺序不一定一致。

![](https://img.mukewang.com/5a66e4d90001feff06580490.png "非聚集索引示意图")

[![](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing3.png "indexing3")](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing3.png)

It requires more time as compared to clustered index because some amount of extra work is done in order to extract the data by further following the pointer. In case of clustered index, data is directly present in front of the index.

在SQL Server里面查询效率如下所示，Index Seek就是索引所花费的开销，Key Lookup就是二次查询所花费的开销。可以看的出二次查询所花费的查询开销占比很大，达到50%。

![](https://img.mukewang.com/5a66e4e50001422504780174.png "查询效率")

在SQL Server里面会对查询自动优化，选择适合的索引，因此如果在数据量不大的情况下，SQL Server很有可能不会使用非聚集索引进行查询，而是使用聚集索引进行查询，即便需要扫描整个聚集索引，效率也比使用非聚集索引效率要高。

![](https://img.mukewang.com/5a66e4f70001748304160102.png "查询效率")

本人试过在含有30w行表上建立非聚集索引，查询非聚集索引覆盖以外的列就会变成聚集索引的全索引扫描（index scan）查询来避免二次查询，而在另外一张200w行表才会用到非聚集索引seek对应的列再进行kek lookup，有关于SQL Server的有Index seek，index scan, table scan，key LookUp这几个概念，可以查看这个[blog](http://www.cnblogs.com/xwdreamer/archive/2012/07/06/2579504.html)，描写比较详细。

但在MySQL里面就算表里数据量少且查询了非键列，也不会使用聚集索引去全索引扫描，但如果强制使用聚集索引去查询，性能反而比非聚集索引查询要差，这就是两种SQL的不同之处。

还有一点要注意的是非聚集索引其实叶子节点除了会存储索引覆盖列的数据，也会存放聚集索引所覆盖的列数据。

### 如何解决非聚集索引的二次查询问题

#### 复合索引（覆盖索引）

建立两列以上的索引，即可查询复合索引里的列的数据而不需要进行回表二次查询，如index\(col1, col2\)，执行下面的语句

```
select
 col1
,
 col2 
from
 t1 
where
 col1 
=
'213'
;
```

要注意使用复合索引需要满足最左侧索引的原则，也就是查询的时候如果where条件里面没有最左边的一到多列，索引就不会起作用。

在SQL Server中还有include的用法，可以把非聚集索引里包含的列包含进来，而不一定需要建立复合索引。

**四.总结与使用心得**

1. 使用聚集索引的查询效率要比非聚集索引的效率要高，但是如果需要频繁去改变聚集索引的值，写入性能并不高，因为需要移动对应数据的物理位置。
2. 非聚集索引在查询的时候可以的话就避免二次查询，这样性能会大幅提升。
3. 不是所有的表都适合建立索引，只有数据量大表才适合建立索引，且建立在选择性高的列上面性能会更好。

## how to implement  index



