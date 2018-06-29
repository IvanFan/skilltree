DBMS: database management system

**1\) Processing Queries and Object Management**:  
In traditional file systems, we cannot store data in the form of objects. In practical-world applications, data is stored in objects and not files. So in a file system, some application software maps the data stored in files to objects so that can be used further.  
We can directly store data in the form of objects in a database management system. Application level code needs to be written to handle, store and scan through the data in a file system whereas a DBMS gives us the ability to query the database.

* **2\) Controlling redundancy and inconsistency:**  
  Redundancy refers to repeated instances of the same data. A database system provides redundancy control whereas in a file system, same data may be stored multiple times. For example, if a student is studying two different educational programs in the same college, say ,Engineering and History, then his information such as the phone number and address may be stored multiple times, once in Engineering dept and the other in History dept. Therefore, it increases time taken to access and store data. This may also lead to inconsistent data states in both places. A DBMS uses**data normalization**to avoid redundancy and duplicates.

* **3\) Efficient memory management and indexing:**  
  DBMS makes complex memory management easy to handle. In file systems, files are indexed in place of objects so query operations require entire file scans whereas in a DBMS , object indexing takes place efficiently through database schema based on any attribute of the data or a data-property. This helps in fast retrieval of data based on the indexed attribute.

* **4\) Concurrency control and transaction management:**  
  Several applications allow user to simultaneously access data. This may lead to inconsistency in data in case files are used. Consider two withdrawal transactions X and Y in which an amount of 100 and 200 is withdrawn from an account A initially containing 1000. Now since these transactions are taking place simultaneously, different transactions may update the account differently. X reads 1000, debits 100, updates the account A to 900, whereas X also reads 1000, debits 200, updates A to 800. In both cases account A has wrong information. This results in data inconsistency. A DBMS provides mechanisms to deal with this kind of data inconsistency while allowing users to access data concurrently. A DBMS implements[ACID](http://quiz.geeksforgeeks.org/acid-properties-in-dbms/)\(atomicity, durability, isolation,consistency\) properties to ensure efficient transaction management without data corruption.

* **5\) Access Control and ease in accessing data:**  
  A DBMS can grant access to various users and determine which part and how much of the data can they access from the database thus removing redundancy. Otherwise in file system, separate files have to be created for each user containing the amount of data that they can access. Moreover, if a user has to extract specific data, then he needs a code/application to process that task in case of file system, e.g. Suppose a manager needs a list of all employees having salary greater than X. Then we need to write business logic for the same in case data is stored in files. In case of DBMS, it provides easy access of data through queries, \(e.g.,**SELECT**queries\) and whole logic need not be rewritten. Users can specify exactly what they want to extract out of the data.

## DBMS 3-Tier Architecture

DBMS 3-tier architecture divides the complete system into three inter-related but independent modules as shown in Figure 1.

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/dbms-3tier.jpg "dbms-3-tier-architecture")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/dbms-3tier.jpg)

**Physical Level:**At physical level, the information about location of database objects in data store is kept. Various users are DBMS are unaware about the locations of these objects. This is the lowest level of data abstraction. It tells us how the data is actually stored in memory. The access methods like sequential or random access and file organisation methods like B+ trees, hashing used for the same. Usability, size of memory, and the number of times the records are factors which we need to know while designing the database.

Suppose we need to store the details of an employee. Blocks of storage and the amount of memory used for these purposes is kept hidden from the user.

**Conceptual Level:**

At conceptual level, data is represented in the form of various database tables. For Example, STUDENT database may contain STUDENT and COURSE tables which will be visible to users but users are unaware about their storage. This level comprises of the information that is actually stored in the database in the form of tables. It also stores the relationship among the data entities in relatively simple structures. At this level, the information available to the user at the view level is unknown.

We can store the various attributes of an employee and relationships, e.g. with the manager can also be stored.

**External Level:** An external level specifies a view of the data in terms of conceptual level tables.  Each external level view is used to cater the needs of a particular category of users. For Example, FACULTY of a university is interested in looking course details of students, STUDENTS are interested in looking all details related to academics, accounts, courses and hostel details as well. So, different views can be generated for different users.

**Data Independence**

Data independence means change of data at one level should not affect another level. Two types of data independence are required in this architecture:

**Physical Data Independence:**Any change in physical location of tables and indexes should not affect conceptual level or external view of data. This data independence is easy to achieve and implemented by most of the DBMS. It refers to the characteristic of being able to modify the physical schema without any alterations to the conceptual or logical schema, done for optimisation purposes, e.g., Conceptual structure of the database would not be affected by any change in storage size of the database system server. Changing from sequential to random access files is one such example.These alterations or modifications to the physical structure may include:

* Utilising new storage devices.
* Modifying data structures used for storage.
* Altering indexes or using alternative file organisation techniques etc. 

**Conceptual Data Independence:**

The data at conceptual level schema and external level schema must be independent. This means, change in conceptual schema should not affect external schema. e.g.; Adding or deleting attributes of a table should not affect the user’s view of table. But this type of independence is difficult to achieve as compared to physical data independence because the changes in conceptual schema are reflected in user’s view.It refers characteristic of being able to modify the logical schema without affecting the external schema or application program. The user view of the data would not be affected by any changes to the conceptual view of the data. These changes may include insertion or deletion of attributes, altering table structures entities or relationships to the logical schema etc.

**Phases of database design**

Database designing for a real world application starts from capturing the requirements to physical implementation using DBMS software which consists of following steps shown in Figure 2.

**Conceptual Design:**The requirements of database are captured using high level conceptual data model. For Example, ER model is used for conceptual design of database.

**Logical Design:**Logical Design represents data in the form of relational model. ER diagram produced in conceptual design phase is used to convert the data into Relational Model.

**Physical Design:**In physical design, data in relational model is implemented using commercial DBMS like Oracle, DB2.

DBMS helps in efficient organization of data in database which has following advantages over typical file system.

* **Minimized redundancy and data consistency:**  
  Data is normalized in DBMS to minimize the redundancy which helps in keeping data consistent. For Example, student information can be kept at one place in DBMS and accessed by different users.

* **Simplified Data Access:**  
  A user need only name of the relation not exact location to access data, so the process is very simple.

* **Multiple data views:**  
  Different views of same data can be created to cater the needs of different users. For Example, faculty salary information can be hidden from student view of data but shown in admin view.

* **Data Security:**  
  Only authorized users are allowed to access the data in DBMS. Also, data can be encrypted by DBMS which makes it secure.

* **Concurrent access to data:**  
  Data can be accessed concurrently by different users at same time in DBMS.

* **Backup and Recovery mechanism:**  
  DBMS backup and recovery mechanism helps to avoid data loss and data inconsistency in case of catastrophic failures.

## Database Objects

A**database object**is any defined object in a database that is used to store or reference data.Anything which we make from**create command**is known as Database Object.It can be used to hold and manipulate the data.Some of the examples of database objects are : view, sequence, indexes, etc.

* **Table –**
  Basic unit of storage; composed rows and columns
* **View –**
  Logically represents subsets of data from one or more tables
* **Sequence –**
  Generates primary key values
* **Index –**
  Improves the performance of some queries
* **Synonym –**
  Alternative name for an objec

**Table –**

This database object is used to create a table in database.

**Syntax :**

```
CREATE TABLE [schema.]table
               (column datatype [DEFAULT expr][, ...]);
```

**Example :**

```
CREATE TABLE dept
           (deptno NUMBER(2),
            dname VARCHAR2(14),
            loc VARCHAR2(13));
```

**Output :**DESCRIBE dept;

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/table-output.png "table output")

1. **View –**  
   This database object is used to create a view in database.A view is a logical table based on a table or another view. A view contains no data of its own but is like a window through which data from tables can be viewed or changed. The tables on which a view is based are called base tables. The view is stored as a SELECT statement in the data dictionary.  
   **Syntax :**

   ```
   CREATE [OR REPLACE] [FORCE|NOFORCE] VIEW view
                          [(alias[, alias]...)]
                          AS subquery
                          [WITH CHECK OPTION [CONSTRAINT constraint]]
                          [WITH READ ONLY [CONSTRAINT constraint]];
   ```

   **Example :**

   ```
   CREATE VIEW salvu50
                  AS SELECT employee_id ID_NUMBER, last_name NAME,
                  salary*12 ANN_SALARY
                  FROM employees
                  WHERE department_id = 50;
   ```

   **Output :**

   ```
   SELECT *
   FROM salvu50;
   ```

   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/view-output.png "view output")

**Index –**

This database object is used to create a indexes in database.An Oracle server index is a schema object that can speed up the retrieval of rows by using a pointer.Indexes can be created explicitly or automatically. If you do not have an index on the column, then a full table scan occurs.

An index provides direct and fast access to rows in a table. Its purpose is to reduce the necessity of disk I/O by using an indexed path to locate data quickly. The index is used and maintained automatically by the Oracle server. Once an index is created, no direct activity is required by the user.Indexes are logically and physically independent of the table they index. This means that they can be created or dropped at any time and have no effect on the base tables or other indexes.

**Syntax :**

```
CREATE INDEX index
            ON table (column[, column]...);
```

**Example :**

```
CREATE INDEX emp_last_name_idx
                ON  employees(last_name);
```



