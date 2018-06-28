**Q \#1\) What do you understand by ‘Database’?**

**Ans:**Database is an organized collection of related data where the data is stored and organized to serve some specific purpose.

For_**Example**_, A librarian maintains a database of all the information related to the books that are available in the library.

**Q \#2\) Define DBMS.**

**Ans:**DBMS stands for Database Management system. It is a collection of application programs which allow the user to organize, restore and retrieve information about data efficiently and as effectively as possible.

Some of the popular DBMS’s are MySql, Oracle, Sybase, etc.

**Q \#3\) Define RDBMS.**

**Ans:**Relational Database Management System\(RDBMS\) is based on a relational model of data that is stored in databases in separate tables and they are related to the use of a common column. Data can be accessed easily from the relational database using Structured Query Language \(SQL\).

**Q \#4\) Enlist the advantages of DBMS.**

**Ans: The Advantages of DBMS includes:**

* Data is stored in a structured way and hence redundancy is controlled.
* Validates the data entered and provide restrictions on unauthorized access to the database.
* Provides backup and recovery of the data when required.
* Provides multiple user interfaces.

**Q \#5\) What do you understand by Data Redundancy?**

**Ans:**Duplication of data in the database is known as Data redundancy. As a result of Data Redundancy, duplicated data is present at various locations, hence it leads to wastage of the storage space and the integrity of the database is destroyed.

**Q \#6\) What are the various types of relationships in Database? Define them.**

**Ans: There are 3 types of relationships in Database:**

* **One-to-one:**
  One table has the relationship with another table having the similar kind of column. Each primary key relates to only one or no record in the related table.
* **One-to-many:**
  One table has a relationship with another table that has primary and foreign key relation. The primary key table contains only one record that relates to none, one or many records in the related table.
* **Many-to-many:**
  Each record in both the tables can relate to many numbers of record in another table.

**Q \#7\) Explain Normalization and De-Normalization.**

**Ans**_**:**_**Normalization**is the process of removing the redundant data from the database by splitting the table in a well-defined manner in order to maintain data integrity. This process saves much of the storage space.

**De-normalization**is the process of adding up redundant data on the table in order to speed up the complex queries and thus achieve better performance.

**Q \#8\) What are the different types of Normalization?**

**Ans: Different Types of Normalization are:**

* **First Normal Form \(1NF\):**
  A relation is said to be in 1NF only when all the entities of the table contain unique or atomic values.
* **Second Normal Form \(2NF\):**
  A relation is said to be in 2NF only if it is in 1NF and all the non-key attribute of the table is fully dependent on the primary key.
* _**Third Normal Form \(3NF\):**_
  A relation is said to be in 3NF only if it is in 2NF and every non-key attribute of the table is not transitively dependent on the primary key.

### 第一范式（1NF）

强调的是

**列的原子性**

，即列不能够再分成其他几列。

考虑这样一个表：【联系人】（姓名，性别，电话）

如果在实际场景中，一个联系人有家庭电话和公司电话，那么这种表结构设计就没有达到 1NF。要符合 1NF 我们只需把列（电话）拆分，即：【联系人】（姓名，性别，家庭电话，公司电话）。1NF 很好辨别，但是 2NF 和 3NF 就容易搞混淆。

说明：在任何一个[关系数据库](http://baike.baidu.com/view/68348.htm)中，第一范式（1NF）是对[关系模式](http://baike.baidu.com/view/68347.htm)的设计基本要求，一般设计中都必须满足第一范式（1NF）。不过有些[关系模型](http://baike.baidu.com/view/176484.htm)中突破了1NF的限制，这种称为非1NF的关系模型。换句话说，是否必须满足1NF的最低要求，主要依赖于所使用的[关系模型](http://baike.baidu.com/view/176484.htm)。

### 第二范式（2NF）

首先是 1NF，另外包含两部分内容，一是表必须有一个主键；二是没有包含在主键中的列必须完全依赖于主键，而不能只依赖于主键的一部分。

考虑一个订单明细表：【OrderDetail】（OrderID，ProductID，UnitPrice，Discount，Quantity，ProductName）。

因为我们知道在一个订单中可以订购多种产品，所以单单一个 OrderID 是不足以成为主键的，主键应该是（OrderID，ProductID）。显而易见 Discount（折扣），Quantity（数量）完全依赖（取决）于主键（OderID，ProductID），而 UnitPrice，ProductName 只依赖于 ProductID。所以 OrderDetail 表不符合 2NF。不符合 2NF 的设计容易产生冗余数据。

可以把【OrderDetail】表拆分为【OrderDetail】（OrderID，ProductID，Discount，Quantity）和【Product】（ProductID，UnitPrice，ProductName）来消除原订单表中UnitPrice，ProductName多次重复的情况。

第二范式（2NF）要求实体的属性**完全依赖**于主关键字。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性，如果存在，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与原实体之间是一对多的关系。为实现区分通常需要为表加上一个列，以存储各个实例的唯一标识。简而言之，第二范式就是在第一范式的基础上属性完全依赖于主键。

### 第三范式（3NF）

在1NF基础上，任何非主[**属性**](http://baike.baidu.com/view/77730.htm)不依赖于其它非主属性\[在2NF基础上消除传递依赖\]。

第三范式（3NF）是第二范式（2NF）的一个子集，即满足第三范式（3NF）必须满足第二范式（2NF）。

首先是 2NF，另外非主键列必须**直接依赖**于主键，不能存在传递依赖。即不能存在：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键的情况。 考虑一个订单表【Order】（OrderID，OrderDate，CustomerID，CustomerName，CustomerAddr，CustomerCity）主键是（OrderID）。

其中 OrderDate，CustomerID，CustomerName，CustomerAddr，CustomerCity 等非主键列都完全依赖于主键（OrderID），所以符合 2NF。不过问题是 CustomerName，CustomerAddr，CustomerCity 直接依赖的是 CustomerID（非主键列），而不是直接依赖于主键，它是通过传递才依赖于主键，所以不符合 3NF。

通过拆分【Order】为【Order】（OrderID，OrderDate，CustomerID）和【Customer】（CustomerID，CustomerName，CustomerAddr，CustomerCity）从而达到 3NF。

第二范式（2NF）和第三范式（3NF）的概念很容易混淆，区分它们的关键点在于，2NF：非主键列是否完全依赖于主键，还是依赖于主键的一部分；3NF：非主键列是直接依赖于主键，还是直接依赖于非主键列。

**Q \#9\) What is BCNF?**

**Ans:**BCNF is Boyce Code Normal form. It is the higher version of 3Nf which does not have any multiple overlapping candidate keys.

**Q \#10\) What is SQL?**

**Ans:**Structured Query language, SQL is an ANSI\(American National Standard Institute\) standard programming language which is designed specifically for storing and managing the data in the relational database management system \(RDBMS\) using all kinds of data operations.

**Q \#11\) How many SQL statements are used? Define them.**

**Ans:**SQL statements are basically divided into three categories, DDL, DML, and DCL.

**They can be defined as:**

**Data Definition Language \(DDL\)**commands are used to define the structure that holds the data. These commands are auto-committed i.e. changes done by the DDL commands on the database are saved permanently.

**Data Manipulation Language \(DML\)**commands are used to manipulate the data of the database. These commands are not auto-committed and can be rolled back.

**Data Control Language \(DCL\)**commands are used to control the visibility of the data in the database like revoke access permission for using data in the database.

**Q \#12\) Enlist some commands of DDL, DML, and DCL.**

**Ans: Data Definition Language \(DDL\) commands:**

* CREATE to create a new table or database.
* ALTER for alteration.
* Truncate to delete data from the table.
* DROP to drop a table.
* RENAME to rename a table.

**Data Manipulation Language \(DML\) commands:**

* INSERT to insert a new row.
* UPDATE to update an existing row.
* DELETE to delete a row.
* MERGE for merging two rows or two tables.

**Data Control Language \(DCL\) commands:**

* COMMIT to permanently save.
* ROLLBACK to undo the change.
* SAVEPOINT to save temporarily.

**Q \#13\) Define DML Compiler.**

**Ans:**DML compiler translates DML statements in a query language into a low-level instruction and the generated instruction can be understood by Query Evaluation Engine.

**Q \#14\) What is DDL interpreter?**

**Ans:**DDL Interpreter interprets the DDL statements and records the generated statements in the table containing metadata.

**Q \#15\) Enlist the advantages of SQL.**

**Ans: Advantages of SQL are:**

* Simple SQL queries can be used to retrieve a large amount of data from the database very quickly and efficiently.
* SQL is easy to learn and almost every DBMS supports SQL.
* It is easier to manage the database using SQL as no large amount of coding is required.

**Q \#16\) Explain the terms ‘Record’, ‘Field’ and ‘Table’ in terms of database.**

**Ans: Record:**Record is a collection of values or fields of a specific entity. Eg. An employee, Salary account, etc.

**Field:**A field refers to an area within a record which is reserved for a specific piece of data. Eg. Employee ID.

**Table:**Table is the collection of records of specific types. E.g. Employee table is a collection of record related to all the employees.

**Q \#17\) What do you understand by Data Independence? What are its two types?**

**Ans:**Data Independence refers to the ability to modify the schema definition in one level in such a way that it does not affect the schema definition in the next higher level.

**The 2 types of Data Independence are:**

* **Physical Data Independence**
  : It modifies the schema at the physical level without affecting the schema at the conceptual level.
* **Logical Data Independence:**
  It modifies the schema at the conceptual level without affecting or causing changes in the schema at the view level.

**Q \#18\) Define the relationship between ‘View’ and ‘Data Independence’.**

**Ans:**View is a virtual table which does not have its data on its own rather the data is defined from one or more underlying base tables.

Views account for logical data independence as the growth and restructuring of base tables is not reflected in views.

**Q \#19\) What are the advantages and disadvantages of views in the database?**

**Ans: Advantages of Views:**

* As there is no physical location where the data in views is stored, it generates output without wasting resources.
* Data access is restricted as it does not allow commands like insertion, updation, and deletion.

**Disadvantages of Views:**

* View becomes irrelevant if we drop a table related to that view.
* More memory is occupied when the view is created for large tables.

**Q \#20\) What do you understand by Functional dependency?**

**Ans:**A relation is said to be in Functional dependency when one attribute uniquely defines another attribute.

**For Example,**R is a Relation, X and Y are two attributes. T1 and T2 are two tuples. Then,

T1\[X\]=T2\[X\] and T1\[Y\]=T2\[Y\] means the value of component X uniquely define the value of component Y.

Also, X-&gt;Y means Y is functionally dependent on X.

**Q \#21\) When is functional dependency said to be the fully functional dependency?**

**Ans:**To fulfill the criteria of fully functional dependency, the relation must meet the requirement of functional dependency.

A functional dependency ‘A’ and ‘B’ is said to be fully functional dependent when removal of any attribute say ‘X’ from ‘A’ means the dependency does not hold anymore.

**Q \#22\) What do you understand by E-R model?**

**Ans:**E-R model is an Entity-Relationship model which defines the conceptual view of the database.

E-R model basically shows the real world entities and their association/relations. Entities here represent the set of attributes in the database.

**Q \#23\) Define Entity, Entity type, and Entity set.**

**Ans: Entity**can be anything, be it a place, class or object which has an independent existence in the real world.

**Entity type**represents a set of entities which have similar attributes.

**Entity set**in the database represents a collection of entities having a particular entity type.

**Q \#24\) Define Weak Entity set.**

**Ans:**Weak entity set is the one whose primary key comprises of its partial key as well as the primary key of its parent entity.

This is the case because the entity set may not have sufficient attributes to form a primary key.

**Q \#25\) Explain the terms ‘Attribute’ and ‘Relations’**

**Ans: Attribute**describes the properties or characteristics of an entity. For_**Example**_, Employee ID, Employee Name, Age, etc., can be attributes of the entity Employee.

**Relation**is a two-dimensional table containing a number of rows and columns where every row represents a record of the relation. Here, rows are also known as ‘Tuples’ and columns are known as ‘Attributes’.

**Q \#26\) What are VDL and SDL?**

**Ans: VDL**is View Definition language which represents user views and their mapping to the conceptual schema.

**SDL**is Storage Definition Language which specifies the mapping between two schemas.

**Q \#27\) Define Cursor and its types.**

**Ans:**Cursor is a temporary work area which stores the data as well as the result set occurred after manipulation of data retrieved. A cursor can hold only one row at a time.

**The 2 types of Cursor are:**

**Implicit cursors**are declared automatically when DML statements like INSERT, UPDATE, DELETE is executed.

**Explicit cursors**have to be declared when SELECT statements which are returning more than one row are executed.

**Q \#28\) What is Database transaction?**

**Ans:**Sequence of operation performed which changes the consistent state of the database to another is known as the database transaction. After the completion of the transaction, either the successful completion is reflected in the system or the transaction fails and no change is reflected.

**Q \#29\) Define Database Lock and its types.**

**Ans:**Database lock basically signifies the transaction about the current status of the data item i.e. whether that data is being used by other transactions or not at the present point of time.

There are two types of Database lock which are**Shared Lock and Exclusive Lock.**

**Q \#30\) What is Data Warehousing?**

**Ans:**The storage as well as access to data, that is being derived from the transactions and other sources, from a central location in order to perform the analysis is called Data Warehousing.

**Q \#31\) What do you understand by Join?**

**Ans:**Join is the process of explaining the relationship between different tables by combining columns from one or more table having common values in each. When a table joins with itself, it is known as Self Join.

**Q \#32\) What do you understand by Index hunting?**

**Ans:**Index hunting is the process of boosting the collection of indexes which help in improving the query performance as well as the speed of the database.

**Q \#33\) How to improve query performance using Index hunting?**

**Ans: Index hunting help in improving query performance by:**

* Using query optimizer to coordinate queries with the workload.
* Observing the performance and effect of index and query distribution.

**Q \#34\) Differentiate between ‘Cluster’ and ‘Non-cluster’ index.**

**Ans:**Clustered Index alters the table and reorders the way in which the records are stored in the table. Data retrieval is made faster by using the clustered index.

A Non-clustered index does alter the records that are stored in the table but creates a completely different object within the table.

**Q \#35\) What are the disadvantages of a Query?**

**Ans: Disadvantages of a Query are:**

* Indexes are not present.
* Stored procedures are excessively compiled.
* Difficulty in interfacing.

**Q \#36\) What do you understand by Fragmentation?**

**Ans:**Fragmentation is a feature which controls the logical data units, also known as fragments that are stored at different sites of a distributed database system.

**Q \#37\) Define Join types.**

**Ans:**Given below are the types of Join, which are explained with respect to the tables as an_**Example**_:

**employee table:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee-table.jpg "employee table")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee-table.jpg)

**employee\_info table:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee_info-table.jpg "employee\_info table")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee_info-table.jpg)

**1\) Inner JOIN:**Inner JOIN is also known as a simple JOIN. This SQL query returns result from both the tables having a common value in rows.

[https://www.w3resource.com/sql/joins/perform-an-inner-join.php](https://www.w3resource.com/sql/joins/perform-an-inner-join.php)

**SQL Query:**

SELECT \* from employee, employee\_info WHERE employee.EmpID = employee\_info.EmpID ;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/02/Inner-Join-Example.jpg "Inner Join Example")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/02/Inner-Join-Example.jpg)

**2\) Natural JOIN:**This is a type of Inner JOIN which returns results from both the tables having same data values in the columns of both the tables to be joined.

**SQL Query:**

SELECT \* from employee NATURAL JOIN employee\_info;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Natural-JOIN.jpg "Natural JOIN")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Natural-JOIN.jpg)

**3\) Cross JOIN:**Cross JOIN return results as all the records where each row from the first table is combined with each row of the second table.

**SQL Query:**

SELECT \* from employee CROSS JOIN employee\_info;

**Result:**

Let us do some modification in the above tables to understand Right JOIN, Left JOIN, and Full JOIN.

**employee table:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee-table-new.jpg "employee table new")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee-table-new.jpg)

**employee\_info table:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee_info-table-new.jpg "employee\_info table new")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employee_info-table-new.jpg)

**1\) Right JOIN:**Right JOIN is also known as Right Outer JOIN. This returns all the rows as a result from the right table even if the JOIN condition does not match any records in the left table.

**SQL Query:**

SELECT \* from employee RIGHT OUTER JOIN employee\_info on \(employee.EmpID = employee\_info.EmpID\);

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/02/Right-Join-Example.jpg "Right Join Example")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/02/Right-Join-Example.jpg)

**2\) Left JOIN:**Left JOIN is also known as Left Outer JOIN. This returns all the rows as a result of the left table even if JOIN condition does not match any records in the right table. This is exactly the opposite of Right JOIN.

**SQL Query:**

SELECT \* from employee LEFT OUTER JOIN employee\_info on \(employee.EmpID = employee\_info.EmpID\);

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Left-JOIN.jpg "Left JOIN")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Left-JOIN.jpg)

**3\) Outer/Full JOIN:**Full JOIN return results in combining the result of both the Left JOIN and Right JOIN.

**SQL Query:**

SELECT \* from employee FULL OUTER JOIN employee\_info on \(employee.EmpID = employee\_info.EmpID\);

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Outer-Full-JOIN.jpg "Outer Full JOIN")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Outer-Full-JOIN.jpg)

**Q \#38\) What do you understand by ‘Atomicity’ and ‘Aggregation’?**

**Ans: Atomicity**is the condition where either all the actions of the transaction are performed or none. This means, when there is an incomplete transaction, database management system itself will undo the effects done by the incomplete transaction.

**Aggregation**is the concept of expressing the relationship with the collection of entities and their relationships.

**Q \#39\) Define Phantom deadlock.**

**Ans:**Phantom deadlock detection is the condition where the deadlock does not actually exist but due to a delay in propagating local information, deadlock detection algorithms identify the deadlocks.

**Q \#40\) Define checkpoint.**

**Ans:**Checkpoint declares a point before which all the logs are stored permanently in the storage disk and is the inconsistent state. In the case of crashes, the amount of work and time is saved as the system can restart from the checkpoint.

**Q \#41\) What is Database partitioning?**

**Ans:**Database partitioning is the process of partitioning tables, indexes into smaller pieces in order to manage and access the data at a finer level.

This process of partitioning reduces the cost of storing a large amount of data as well as enhances the performance and manageability.

**Q \#42\) Explain the importance of Database partitioning.**

**Ans: The importance of Database partitioning are:**

* Improves query performance and manageability.
* Simplifies common administration tasks.
* Acts as a key tool for building systems with extremely high availability requirements.
* Allows accessing a large part of a single partition.

**Q \#43\) Explain Data Dictionary.**

**Ans:**Data dictionary is a set of information describing the content and structure of the tables and database objects. The job of the information stored in the data dictionary is to control, manipulate and access the relationship between database elements.

**Q \#44\) Explain Primary Key and Composite Key.**

**Ans: Primary key**is that column of the table whose every row data is uniquely identified. Every row in the table must have a primary key and no two rows can have the same primary key. Primary key value can never be null nor can be modified or updated.

**Composite Key**is a form of the candidate key where a set of columns will uniquely identify every row in the table.

**Q \#45\) What do you understand by Unique key?**

**Ans:**A Unique key is same as the primary key whose every row data is uniquely identified with a difference of null value i.e. Unique key allows one value as NULL value.

**Q \#46\) What do you understand by Database Triggers?**

**Ans:**A set of commands that automatically get executed when an event like Before Insert, After Insert, On Update, On Delete of row occurs in a table is called as Database trigger.

**Q \#47\) Define Stored procedures.**

**Ans:**A Stored procedure is a collection of pre-compiled SQL Queries, which when executed denotes a program taking input, process and gives the output.

**Q \#48\) What do you understand by B-Trees?**

**Ans:**B-Tree represents the data structure in the form of a tree for external memory that reads and writes large blocks of data. It is commonly used in databases and file systems where all the insertions, deletions, sorting, etc., are done in logarithmic time.

**Q \#49\) Name the different data models that are available for database systems.**

**Ans: Different data models are:**

* Relational model
* Network model
* Hierarchical model

**Q \#50\) Differentiate between ‘DELETE’, ‘TRUNCATE’ and ‘DROP’ commands.**

**Ans:**After the execution of**‘DELETE’**operation, COMMIT and ROLLBACK statements can be performed to retrieve the lost data.

After the execution of**‘TRUNCATE’**operation, COMMIT, and ROLLBACK statements cannot be performed to retrieve the lost data.

**‘DROP’**command is used to drop the table or key like the primary key/foreign key.

**Q \#51\) Based on the given table, solve the following queries.**

**Employee table**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Employee-table-1.jpg "Employee table 1")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Employee-table-1.jpg)

**1\)**Write the SELECT command to display the details of the employee with empid as 1004.

**Ans:**

SELECT empId, empName, Age, Address from Employee WHERE empId = 1004;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/SELECT-command.jpg "SELECT command")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/SELECT-command.jpg)

**2\)**Write the SELECT command to display all the records of table Employee.

**Ans:**

SELECT \* from Employee;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/display-all-records.jpg "display all records")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/display-all-records.jpg)

**3\)**Write the SELECT command to display all the records of the employee whose name starts with the character ‘R’.

**Ans:**

SELECT \* from Employee WHERE empName LIKE ‘R%’;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/name-starts-with-character-R.jpg "name starts with character R")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/name-starts-with-character-R.jpg)

**4\)**Write a SELECT command to display id, age and name of the employees with their age in both ascending and descending order.

**Ans:**

SELECT empId, empName, Age from Employee  ORDER BY Age;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employees-with-their-age-in-ascending.jpg "employees with their age in ascending")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employees-with-their-age-in-ascending.jpg)

SELECT empId, empName, Age from Employee  ORDER BY Age Desc;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employees-with-their-age-in-descending.jpg "employees with their age in descending")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/employees-with-their-age-in-descending.jpg)

**5\)**Write the SELECT command to calculate the total amount of salary on each employee from the below Emp table.

**Emp table:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Emp-table-1-e1492776699935.jpg "Emp table")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Emp-table-1.jpg)

**Ans:**

SELECT empName, SUM\(Salary\) from Emp GROUP BY empName;

**Result:**

[![](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Result.jpg "Result")](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/04/Result.jpg)

### Conclusion

These are the set of Database interview questions and answers which are mostly asked in the interview.

Mostly the basics of every subject are questioned in the interviews. It is a well-known fact to everyone that, if your basics are clear, you can reach top heights.

