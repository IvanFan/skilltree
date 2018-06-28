DBMS: database management system

## DBMS 3-Tier Architecture

DBMS 3-tier architecture divides the complete system into three inter-related but independent modules as shown in Figure 1.

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/dbms-3tier.jpg "dbms-3-tier-architecture")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/dbms-3tier.jpg)

**Physical Level:**At physical level, the information about location of database objects in data store is kept. Various users are DBMS are unaware about the locations of these objects.

  
**Conceptual Level:**

At conceptual level, data is represented in the form of various database tables. For Example, STUDENT database may contain STUDENT and COURSE tables which will be visible to users but users are unaware about their storage.



**External Level:** An external level specifies a view of the data in terms of conceptual level tables.  Each external level view is used to cater the needs of a particular category of users. For Example, FACULTY of a university is interested in looking course details of students, STUDENTS are interested in looking all details related to academics, accounts, courses and hostel details as well. So, different views can be generated for different users.

  
**Data Independence**

Data independence means change of data at one level should not affect another level. Two types of data independence are required in this architecture:

**Physical Data Independence:**Any change in physical location of tables and indexes should not affect conceptual level or external view of data. This data independence is easy to achieve and implemented by most of the DBMS.

**Conceptual Data Independence:**

The data at conceptual level schema and external level schema must be independent. This means, change in conceptual schema should not affect external schema. e.g.; Adding or deleting attributes of a table should not affect the user’s view of table. But this type of independence is difficult to achieve as compared to physical data independence because the changes in conceptual schema are reflected in user’s view.

