ER Model is used to model the logical view of the system from data perspective which consists of these components:

**Entity, Entity Type, Entity Set –**

An Entity may be an object with a physical existence – a particular person, car, house, or employee – or it may be an object with a conceptual existence – a company, a job, or a university course.

An Entity is an object of Entity Type and set of all entities is called as entity set. e.g.; E1 is an entity having Entity Type Student and set of all students is called Entity Set. In ER diagram, Entity Type is represented as:

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model.png)

**Attribute\(s\):**  
Attributes are the**properties which define the entity type**. For example, Roll\_No, Name, DOB, Age, Address, Mobile\_No are the attributes which defines entity type Student. In ER diagram, attribute is represented by an oval.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-2.png "er2")

**Key Attribute –**

1. The attribute which  
   **uniquely identifies each entity**  
   in the entity set is called key attribute.For example, Roll\_No will be unique for each student. In ER diagram, key attribute is represented by an oval with underlying lines.  
   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-3.png "rno")

2. **Composite Attribute –**

   An attribute  
   **composed of many other attribute**  
   is called as composite attribute. For example, Address attribute of student Entity type consists of Street, City, State, and Country. In ER diagram, composite attribute is represented by an oval comprising of ovals.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-4.png "er22")

1. **Multivalued Attribute –**

   An attribute consisting  
   **more than one value**  
   for a given entity. For example, Phone\_No \(can be more than one for a given student\). In ER diagram, multivalued attribute is represented by double oval.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-5.png "pno")

1. **Derived Attribute –**

   An attribute which can be  
   **derived from other attributes**  
   of the entity type is known as derived attribute. e.g.; Age \(can be derived from DOB\). In ER diagram, derived attribute is represented by dashed oval.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-6.png "er6")The complete entity type**Student**with its attributes can be represented as:

[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-7.png "Capture")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-7.png)**Relationship Type and Relationship Set:**  
A relationship type represents the**association between entity types**. For example,‘Enrolled in’ is a relationship type that exists between entity type Student and Course. In ER diagram, relationship type is represented by a diamond and connecting the entities with lines.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-8.png "er8")A set of relationships of same type is known as relationship set. The following relationship set depicts S1 is enrolled in C2, S2 is enrolled in C1 and S3 is enrolled in C3.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-S-ystem-ER-Model-9.png "er9")

**Degree of a relationship set:**  
The number of different entity sets**participating in a relationship**set is called as degree of a relationship set.

1. **Unary Relationship –**

   When there is  
   **only ONE entity set participating in a relation**  
   , the relationship is called as unary relationship. For example, one person is married to only one person.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-10.png "er10")

1. **Binary Relationship –**

   When there are  
   **TWO entities set participating in a relation**  
   , the relationship is called as binary relationship.For example, Student is enrolled in Course.

![](https://contribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-8.png "er11")

1. **n-ary Relationship –**

   When there are n entities set participating in a relation, the relationship is called as n-ary relationship.

**Cardinality:**  
The**number of times an entity of an entity set participates in a relationship**set is known as cardinality. Cardinality can be of different types:

1. **One to one –**  
   When each entity in each entity set can take part  
   **only once in the relationship**  
   , the cardinality is one to one. Let us assume that a male can marry to one female and a female can marry to one male. So the relationship will be one to one.  
   ![](https://contribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-12.png "er20")

   Using Sets, it can be represented as:

   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-13.png "er12")

2. **Many to one –**  
   When entities in one entity set  
   **can take part only once in the relationship set and entities in other entity set can take part more than once in the relationship set,**  
   cardinality is many to one. Let us assume that a student can take only one course but one course can be taken by many students. So the cardinality will be n to 1. It means that for one course there can be n students but for one student, there will be only one course.  
   [![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-14.png "ernew")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-14.png)Using Sets, it can be represented as:

   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-15.png "er14")

   In this case, each student is taking only 1 course but 1 course has been taken by many students.

3. **Many to many –**  
   When entities in all entity sets can  
   **take part more than once in the relationship**  
   cardinality is many to many. Let us assume that a student can take more than one course and one course can be taken by many students. So the relationship will be many to many.  
   ![](https://contribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-16.png "n2")

   Using sets, it can be represented as:

   ![](https://contribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-17.png "er16")

   In this example, student S1 is enrolled in C1 and C3 and Course C3 is enrolled by S1, S3 and S4. So it is many to many relationships.

**Participation Constraint:**  
Participation Constraint is applied on the entity participating in the relationship set.

1. **Total Participation –**
   Each entity in the entity set
   **must participate**
   in the relationship. If each student must enroll in a course, the participation of student will be total. Total participation is shown by double line in ER diagram.
2. **Partial Participation –**  
   The entity in the entity set  
   **may or may NOT participat**  
   e in the relationship. If some courses are not enrolled by any of the student, the participation of course will be partial.  
   The diagram depicts the ‘Enrolled in’ relationship set with Student Entity set having total participation and Course Entity set having partial participation.

   ![](https://contribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-18.png "Capture")

   Using set, it can be represented as,

   ![](https://www.geeksforgeeks.org/wp-content/uploads/33333-1.png "33333")

   Every student in Student Entity set is participating in relationship but there exists a course C4 which is not taking part in the relationship.

**Weak Entity Type and Identifying Relationship:**  
As discussed before, an entity type has a key attribute which uniquely identifies each entity in the entity set. But there exists**some entity type for which key attribute can’t be defined**. These are called Weak Entity type.

For example, A company may store the information of dependants \(Parents, Children, Spouse\) of an Employee. But the dependents don’t have existence without the employee. So Dependent will be weak entity type and Employee will be Identifying Entity type for Dependant.

A weak entity type is represented by a double rectangle. The participation of weak entity type is always total. The relationship between weak entity type and its identifying strong entity type is called identifying relationship and it is represented by double diamond.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-20.png "n3")**Generalization and Specialization –**  
These are very common relationship found in real entities. However this kind of relationships was added later as enhanced extension to classical ER model.**Specialized class**are often called as**subclass**while**generalized class**are called superclass, probably inspired by object oriented programming. A sub-class is best understood by**“IS-A analysis”**. Following statements hopefully makes some sense to your mind “Technician IS-A Employee”, “Laptop IS-A Computer”.

An entity is specialized type/class of other entity. For example, Technician is special Employee in a university system Faculty is special class of Employee. We call this phenomenon as generalization/specialization. In the example here Employee is generalized entity class while Technician and Faculty are specialized class of Employee.

**Example –**This example instance of**“sub-class”**relationships. Here we have four sets employee: Secretary, Technician, and Engineer. Employee is super-class of rest three set of individual sub-class is subset of Employee set.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Enhanced-ER-Model-Diagram.png "Enhanced-ER-Model-Diagram")

  


  


* An entity belonging to a sub-class is related with some super-class entity. For instance emp no 1001 is a secretary, and his typing speed is 68. Emp no 1009 is engineer \(sub-class\) and her trade is “Electrical”, so forth.
* Sub-class entity “inherits” all attributes of super-class; for example employee 1001 will have attributes eno, name, salary, and typing speed.

**Constraints –**There are two types of constraints on “Sub-class” relationship.

1. **Total or Partial –**
   A sub-classing relationship is total if every super-class entity is to be associated with some sub-class entity, otherwise partial. Sub-class “job type based employee category” is partial sub-classing – not necessary every employee is one of \(secretary, engineer, and technician\), i.e. union of these three types is proper subset of all employees. Whereas other sub-classing “Salaried Employee AND Hourly Employee” is total; union of entities from sub-classes is equal to total employee set, i.e. every employee necessarily has to be one of them.
2. **Overlapped or Disjoint –**
   If an entity from super-set can be related \(can occur\) in multiple sub-class sets, then it is overlapped sub-classing, otherwise disjoint. Both the examples: job-type based and salaries/hourly employee sub-classing are disjoint.

**Note –**These constraints are independent of each other: can be “overlapped and total or partial” or “disjoint and total or partial”. Also sub-classing has transitive property.

**Multiple Inheritance \(sub-class of multiple super classes\) –**  
An entity can be sub-class of multiple entity types; such entities are sub-class of multiple entities and have multiple super-classes; Teaching Assistant can subclass of Employee and Student both. A faculty in a university system can be sub-class of Employee and Alumnus both. In multiple inheritance, attributes of sub-class is union of attributes of all super-classes.

**Union –**

* Set of Libray Members is
  **UNION**
  of Faculty, Student, and Staff. A union relationship indicates either of type; for example: a library member is either Faculty or Staff or Student.
* Below are two examples shows how
  **UNION**
  can be depicted in ERD – Vehicle Owner is UNION of PERSON and Company, and RTO Registered Vehicle is UNION of Car and Truck.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Enhanced-ER-Model-Diagram-2.png "Enhanced-ER-Model-Diagram")

You might see some confusion in Sub-class and UNION; consider example in above figure Vehicle is super-class of CAR and Truck; this is very much the correct example of subclass as well but here use it different we are saying RTO Registered vehicle is UNION of Car and Vehicle, they do not inherit any attribute of Vehicle, attributes of car and truck are altogether independent set, where is in sub-classing situation car and truck would be inheriting the attribute of vehicle class. Below is Vehicle as modeled as class of Car and Truck.

Generalization, Specialization and Aggregation in ER model are used for data abstraction in which abstraction mechanism is used to hide details of a set of objects.

**Generalization –**  
Generalization is the process of extracting common properties from a set of entities and create a generalized entity from it. It is a bottom-up approach in which two or more entities can be generalized to a higher level entity if they have some attributes in common. For Example, STUDENT and FACULTY can be generalized to a higher level entity called PERSON as shown in Figure 1. In this case, common attributes like P\_NAME, P\_ADD become part of higher entity \(PERSON\) and specialized attributes like S\_FEE become part of specialized entity \(STUDENT\).  
[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/generalization.png "img1")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/generalization.png)

**Specialization –**  
In specialization, an entity is divided into sub-entities based on their characteristics. It is a top-down approach where higher level entity is specialized into two or more lower level entities. For Example, EMPLOYEE entity in an Employee management system can be specialized into DEVELOPER, TESTER etc. as shown in Figure 2. In this case, common attributes like E\_NAME, E\_SAL etc. become part of higher entity \(EMPLOYEE\) and specialized attributes like TES\_TYPE become part of specialized entity \(TESTER\).  
[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/specialization.png "img2")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/specialization.png)

**Aggregation –**  
An ER diagram is not capable of representing relationship between an entity and a relationship which may be required in some scenarios. In those cases, a relationship with its corresponding entities is aggregated into a higher level entity. For Example, Employee working for a project may require some machinery. So, REQUIRE relationship is needed between relationship WORKS\_FOR and entity MACHINERY. Using aggregation, WORKS\_FOR relationship with its entities EMPLOYEE and PROJECT is aggregated into single entity and relationship REQUIRE is created between aggregated entity and MACHINERY.  
[![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/aggregation.png "img3")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/aggregation.png)

  


