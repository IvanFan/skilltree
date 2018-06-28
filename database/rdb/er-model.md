ER Model is used to model the logical view of the system from data perspective which consists of these components:

**Entity, Entity Type, Entity Set –**

An Entity may be an object with a physical existence – a particular person, car, house, or employee – or it may be an object with a conceptual existence – a company, a job, or a university course.

An Entity is an object of Entity Type and set of all entities is called as entity set. e.g.; E1 is an entity having Entity Type Student and set of all students is called Entity Set. In ER diagram, Entity Type is represented as:

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model.png)

**Attribute\(s\):**  
Attributes are the**properties which define the entity type**. For example, Roll\_No, Name, DOB, Age, Address, Mobile\_No are the attributes which defines entity type Student. In ER diagram, attribute is represented by an oval.

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-2.png "er2")



**Key Attribute –**

1.   The attribute which
   **uniquely identifies each entity**
   in the entity set is called key attribute.For example, Roll\_No will be unique for each student. In ER diagram, key attribute is represented by an oval with underlying lines.
   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-3.png "rno")

2. **Composite Attribute –**
 
   An attribute
   **composed of many other attribute**
   is called as composite attribute. For example, Address attribute of student Entity type consists of Street, City, State, and Country. In ER diagram, composite attribute is represented by an oval comprising of ovals.


   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-4.png "er22")

3. **Multivalued Attribute –**
 
   An attribute consisting
   **more than one value**
   for a given entity. For example, Phone\_No \(can be more than one for a given student\). In ER diagram, multivalued attribute is represented by double oval.


   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-5.png "pno")

4. **Derived Attribute –**
 
   An attribute which can be
   **derived from other attributes**
   of the entity type is known as derived attribute. e.g.; Age \(can be derived from DOB\). In ER diagram, derived attribute is represented by dashed oval.


   ![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-6.png "er6")The complete entity type**Student**with its attributes can be represented as:

   [![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-7.png "Capture")](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/Database-Management-System-ER-Model-7.png)



