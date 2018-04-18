## mutable & immutable

string tuple and number are immutable and list dict set are mutable

It means that when passing immutable attributes in to functions, system will pass a copy with a new address

But if it's a mutable attribute, the address is the same. If we assign new value to the mutable attribute, it will pass a new address.

After than the change for this attribute within the function does not affect the outside attribute. Because the address has been changed

## Metaclass

Before understanding metaclasses, you need to master classes in Python. And Python has a very peculiar idea of what classes are, borrowed from the Smalltalk language.

In most languages, classes are just pieces of code that describe how to produce an object. That's kinda true in Python too:

```
>>> class ObjectCreator(object):
...       pass
...

>>> my_object = ObjectCreator()
>>> print(my_object)
<__main__.ObjectCreator object at 0x8974f2c>
```

But classes are more than that in Python. Classes are objects too.

Yes, objects.

As soon as you use the keyword **class**, Python executes it and creates an OBJECT. The instruction

```
>>> class ObjectCreator(object):
...       pass
...
```

creates in memory an object with the name "ObjectCreator".

**This object \(the class\) is itself capable of creating objects \(the instances\), and this is why it's a class**

But still, it's an object, and therefore:

* you can assign it to a variable
* you can copy it
* you can add attributes to it
* you can pass it as a function parameter

e.g.:

```
>>> print(ObjectCreator) # you can print a class because it's an object
<class '__main__.ObjectCreator'>
>>> def echo(o):
...       print(o)
...
>>> echo(ObjectCreator) # you can pass a class as a parameter
<class '__main__.ObjectCreator'>
>>> print(hasattr(ObjectCreator, 'new_attribute'))
False
>>> ObjectCreator.new_attribute = 'foo' # you can add attributes to a class
>>> print(hasattr(ObjectCreator, 'new_attribute'))
True
>>> print(ObjectCreator.new_attribute)
foo
>>> ObjectCreatorMirror = ObjectCreator # you can assign a class to a variable
>>> print(ObjectCreatorMirror.new_attribute)
foo
>>> print(ObjectCreatorMirror())
<__main__.ObjectCreator object at 0x8997b4c>
```



