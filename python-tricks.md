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

You've seen that  type lets you do something like this:

```
MyClass = type('MyClass', (), {})
```

It's because the function type is in fact a metaclass. type is the metaclass Python uses to create all classes behind the scenes.

Everything, and I mean everything, is an object in Python. That includes ints, strings, functions and classes. All of them are objects. And all of them have been created from a class:

```
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>> foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
```

Now, what is the \_\_class\_\_ of any \_\_class\_\_ ?

```
>>> age.__class__.__class__
<type 'type'>
>>> name.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```

type is the built-in metaclass Python uses, but of course, you can create your own metaclass.

You can add a \_\_metaclass\_\_ attribute when you write a class:

```
class Foo(object):
    __metaclass__ = something...
    [...]
```

If you do so, Python will use the metaclass to create the class Foo

```markdown
You write class Foo(object) first, but the class object Foo is not created in memory yet.
Python will look for __metaclass__ in the class definition. 
If it finds it, it will use it to create the object class Foo. 
If it doesn't, it will use type to create the class.
```

When you do:

```
class Foo(Bar):
    pass
```

Python does the following:

Is there a \_\_metaclass\_\_ attribute in Foo?

If yes, create in memory a class object \(I said a class object, stay with me here\), with the name Fooby using what is in \_\_metaclass\_\_.

If Python can't find \_\_metaclass\_\_, it will look for a \_\_metaclass\_\_ at the MODULE level, and try to do the same \(but only for classes that don't inherit anything, basically old-style classes\).

Then if it can't find any \_\_metaclass\_\_ at all, it will use the Bar's \(the first parent\) own metaclass \(which might be the default type\) to create the class object.

Be careful here that the **\_\_metaclass\_\_ attribute will not be inherited**, the metaclass of the parent \(Bar.\_\_class\_\_\) will be. If Bar used a \_\_metaclass\_\_ attribute that created Bar with type\(\) \(and not type.\_\_new\_\_\(\)\), the subclasses will not inherit that behavior.

Now the big question is, what can you put in \_\_metaclass\_\_ ?

The answer is: something that can create a class.

And what can create a class? type, or anything that subclasses or uses it.



