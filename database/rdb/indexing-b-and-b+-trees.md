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

### **Non­-Clustered Indexing**

​A non clustered index just tells us where the data lies, i.e. it gives us a list of virtual pointers or references to the location where the data is actually stored. Data is not physically stored in the order of the index. Instead , data is present in leaf nodes. For eg. the contents page of a book. Each entry gives us the page number or location of the information stored. The actual data here\(information on each page of book\) is not organised but we have an ordered reference\(contents page\) to where the data points actually lie.

[![](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing3.png "indexing3")](https://www.geeksforgeeks.org/wp-content/uploads/gq/2016/07/indexing3.png)

It requires more time as compared to clustered index because some amount of extra work is done in order to extract the data by further following the pointer. In case of clustered index, data is directly present in front of the index.





