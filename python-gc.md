## GC

As I known, JVM has a complex GC process.

When it comes to Python, we will use similar concepts of GC



### Reference counting

PyObject is a common content for each object. ob\_refcnt is the reference count. If the object has a new reference, the ob\_refcnt will increase. vice versa. If ob\_refcnt is equal to 0, the life of object should end



                             



