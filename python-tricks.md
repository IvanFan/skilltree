## mutable & immutable

string tuple and number are immutable and list dict set are mutable

It means that when passing immutable attributes in to functions, system will pass a copy with a new address

But if it's a mutable attribute, the address is the same. If we assign new value to the mutable attribute, it will pass a new address.

After than the change for this attribute within the function does not affect the outside attribute. Because the address has been changed



## Metaclass

## Before understanding metaclasses, you need to master classes in Python. And Python has a very peculiar idea of what classes are, borrowed from the Smalltalk language.

In most languages, classes are just pieces of code that describe how to produce an object. That's kinda true in Python too:

```
>>> class ObjectCreator(object):
...       pass
...

>>> my_object = ObjectCreator()
>>> print(my_object)
<__main__.ObjectCreator object at 0x8974f2c>
```



