## Singleton

Singleton is a normal design pattern. Singleton can make sure that the class only has one instance. Therefore, we can control the number of instances. If we expect there is only one class instance in the system, we can use singleton.

singleton class can only be instance

\_\_new\_\_\(\)will be called before \_\_init\_\_\(\)   when initing new instance.

### use `__new__`

```py
class Singleton(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kw)
        return cls._instance

class MyClass(Singleton):
    a = 1
```



