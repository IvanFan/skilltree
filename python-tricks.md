## mutable & immutable

string tuple and number are immutable and list dict set are mutable

It means that when passing immutable attributes in to functions, system will pass a copy with a new address

But if it's a mutable attribute, the address is the same. If we assign new value to the mutable attribute, it will pass a new address.

After than the change for this attribute within the function does not affect the outside attribute. Because the address has been changed

## Metaclass

Metaclasses are the 'stuff' that creates classes.

You define classes in order to create objects, right?

But we learned that Python classes are objects.

Well, metaclasses are what create these objects. They are the classes' classes, you can picture them this way:

```
MyClass = MetaClass()
my_object = MyClass()
```

You've seen that  type lets you do something like this:

```
MyClass = type('MyClass', (), {})
```



