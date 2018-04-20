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

### Share attributes

when creating instances, we can direct \_\_dict\_\_ of all instances to the same dict. So that they will have the same attributes

```py
class Borg(object):
    _state = {}
    def __new__(cls, *args, **kw):
        ob = super(Borg, cls).__new__(cls, *args, **kw)
        ob.__dict__ = cls._state
        return ob

class MyClass2(Borg):
    a = 1
```

### Decorator

```py
def singleton(cls, *args, **kw):
    instances = {}
    def getinstance():
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return getinstance

@singleton
class MyClass:
```

### Import

```py
# mysingleton.py
class My_Singleton(object):
    def foo(self):
        pass

my_singleton = My_Singleton()

# to use
from mysingleton import my_singleton

my_singleton.foo()
```





## Go Deep

```
class Singleton(object):
    __instance = None
    def __new__(cls, *args, **kwargs):  # 这里不能使用__init__，因为__init__是在instance已经生成以后才去调用的
        if cls.__instance is None:
            cls.__instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls.__instance

s1 = Singleton()
s2 = Singleton()
print s1
print s2


```

the result is:

```
<__main__.Singleton object at 0x7f3580dbe110>
<__main__.Singleton object at 0x7f3580dbe110>
```

let's go one step further



