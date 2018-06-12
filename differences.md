# isinstance

Use this built-in function to find out if a given object is an instance of a certain class or any of its subclasses. You can even pass a tuple of classes to be checked for the object.

The only gotcha you will discover is this: in Python a bool object is an instance of int! Yes, not kidding!

# issubclass

This built-in function is similar to isinstance, but to check if a type is an instance of a class or any of its subclasses.

Again, the only gotcha is that bool type is subclass of int type.



```
>>> isinstance(True, int)
True
>>> isinstance(True, float)
False
>>> isinstance(3, int)
True
>>> isinstance([1,2,3], list)
True
>>> isinstance("aa", str)
True
>>> isinstance(u"as", unicode)
True
>>> issubclass(bool, int)
True
```



