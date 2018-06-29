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

[**数据库**](http://lib.csdn.net/base/14)**索引**，是数据库管理系统中一个排序的数据结构，以协助快速查询、更新数据库表中数据。**索引的实现通常使用B树及其变种B+树**。

在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。  


为表设置索引要付出代价的：一是增加了数据库的存储空间，二是在插入和修改数据时要花费较多的时间\(因为索引也要随之变动\)。

![](http://my.csdn.net/uploads/201205/03/1336034967_9324.png)  




上图展示了一种可能的索引方式。左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址（注意逻辑上相邻的记录在磁盘上也并不是一定物理相邻的）。为了加快Col2的查找，可以维护一个右边所示的二叉查找树，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找在O\(log2n\)的复杂度内获取到相应数据。

  


创建索引可以大大提高系统的性能。

第一，通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

第二，可以大大加快数据的检索速度，这也是创建索引的最主要的原因。

第三，可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。

第四，在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。

第五，通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。   


  


也许会有人要问：增加索引有如此多的优点，为什么不对表中的每一个列创建一个索引呢？因为，增加索引也有许多不利的方面。

第一，创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。

第二，索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。

第三，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。  


  


索引是建立在数据库表中的某些列的上面。在创建索引的时候，应该考虑在哪些列上可以创建索引，在哪些列上不能创建索引。**一般来说，应该在这些列上创建索引：**在经常需要搜索的列上，可以加快搜索的速度；在作为主键的列上，强制该列的唯一性和组织表中数据的排列结构；在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度；在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其指定的范围是连续的；在经常需要排序的列上创建索引，因为索引已经排序，这样查询可以利用索引的排序，加快排序查询时间；在经常使用在WHERE子句中的列上面创建索引，加快条件的判断速度。  


  


同样，对于有些列不应该创建索引。**一般来说，不应该创建索引的的这些列具有下列特点：**

第一，对于那些在查询中很少使用或者参考的列不应该创建索引。这是因为，既然这些列很少使用到，因此有索引或者无索引，并不能提高查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。

第二，对于那些只有很少数据值的列也不应该增加索引。这是因为，由于这些列的取值很少，例如人事表的性别列，在查询的结果中，结果集的数据行占了表中数据行的很大比例，即需要在表中搜索的数据行的比例很大。增加索引，并不能明显加快检索速度。

第三，对于那些定义为text, image和bit数据类型的列不应该增加索引。这是因为，这些列的数据量要么相当大，要么取值很少。

第四，当修改性能远远大于检索性能时，不应该创建索引。这是因为，**修改性能和检索性能是互相矛盾的**。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能。因此，当修改性能远远大于检索性能时，不应该创建索引。  


  


根据数据库的功能，可以在数据库设计器中创建三种索引：**唯一索引、主键索引和聚集索引**。

唯一索引



唯一索引是不允许其中任何两行具有相同索引值的索引。

当现有数据中存在重复的键值时，大多数数据库不允许将新创建的唯一索引与表一起保存。数据库还可能防止添加将在表中创建重复键值的新数据。例如，如果在employee表中职员的姓\(lname\)上创建了唯一索引，则任何两个员工都不能同姓。

主键索引

数据库表经常有一列或列组合，其值唯一标识表中的每一行。该列称为表的主键。

在数据库关系图中为表定义主键将自动创建主键索引，主键索引是唯一索引的特定类型。该索引要求主键中的每个值都唯一。当在查询中使用主键索引时，它还允许对数据的快速访问。

聚集索引

在聚集索引中，表中行的物理顺序与键值的逻辑（索引）顺序相同。一个表只能包含一个聚集索引。

如果某索引不是聚集索引，则表中行的物理顺序与键值的逻辑顺序不匹配。与非聚集索引相比，聚集索引通常提供更快的数据访问速度。

  


  




### 局部性原理与磁盘预读



由于存储介质的特性，磁盘本身存取就比主存慢很多，再加上机械运动耗费，磁盘的存取速度往往是主存的几百分分之一，因此为了提高效率，要尽量减少磁盘I/O。为了达到这个目的，磁盘往往不是严格按需读取，而是每次都会预读，即使只需要一个字节，磁盘也会从这个位置开始，顺序向后读取一定长度的数据放入内存。这样做的理论依据是计算机科学中著名的**局部性原理**：**当一个数据被用到时，其附近的数据也通常会马上被使用。程序运行期间所需要的数据通常比较集中。**

由于磁盘顺序读取的效率很高（不需要寻道时间，只需很少的旋转时间），因此对于具有局部性的程序来说，预读可以提高I/O效率。

预读的长度一般为页（page）的整倍数。页是计算机管理存储器的逻辑块，硬件及操作系统往往将主存和磁盘存储区分割为连续的大小相等的块，每个存储块称为一页（在许多操作系统中，页得大小通常为4k），主存和磁盘以页为单位交换数据。当程序要读取的数据不在主存中时，会触发一个缺页异常，此时系统会向磁盘发出读盘信号，磁盘会找到数据的起始位置并向后连续读取一页或几页载入内存中，然后异常返回，程序继续运行。

### B-/+Tree索引的性能分析

到这里终于可以分析B-/+Tree索引的性能了。

上文说过一般使用磁盘I/O次数评价索引结构的优劣。先从B-Tree分析，根据B-Tree的定义，可知检索一次最多需要访问h个节点。数据库系统的设计者巧妙利用了磁盘预读原理，将一个节点的大小设为等于一个页，这样每个节点只需要一次I/O就可以完全载入。为了达到这个目的，在实际实现B-Tree还需要使用如下技巧：

每次新建节点时，直接申请一个页的空间，这样就保证一个节点物理上也存储在一个页里，加之计算机存储分配都是按页对齐的，就实现了一个node只需一次I/O。

**B-Tree中一次检索最多需要h-1次I/O（根节点常驻内存），渐进复杂度为O\(h\)=O\(logdN\)。**一般实际应用中，出度d是非常大的数字，通常超过100，因此h非常小（通常不超过3）。

而红黑树这种结构，h明显要深的多。由于逻辑上很近的节点（父子）物理上可能很远，无法利用局部性，所以红黑树的I/O渐进复杂度也为O\(h\)，效率明显比B-Tree差很多。

  


**综上所述，用B-Tree作为索引结构效率是非常高的。**

