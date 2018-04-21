## Closure

```py
def print_msg():
    # print_msg 是外围函数
    msg = "zen of python"
    def printer():
        # printer 是嵌套函数
        print(msg)
    return printer

another = print_msg()
# 输出 zen of python
another()
```

another  example

```py
def adder(x):
    def wrapper(y):
        print(x + y)
    return wrapper

adder5 = adder(5)
# 输出 15
adder5(10)
# 输出 11
adder5(6)
```

Closure is like a package which contains variables. From above examples, we can say that another is a closure. It consists of 2 things: printer and msg. Because of Closure, msg is always saved within memory.

All functions have an attribute called _\*\*\_\_closure\_\_\*\*  .\_ If the function is a Closure, this attribute represents a tuple which consists of cell objects

```py
>>> adder.__closure__
>>> adder5.__closure__
(<cell at 0x103075910: int object at 0x7fd251604518>,)
>>> adder5.__closure__[0].cell_contents
5
```

Scope, Closure and GC are common concept in many programming languages. E.g. javascript and php

They all dynamic programming languages. Since the variables be saved in the memory, the incorrect usage may lead to GC issues.

Decorator is an implementation of closure concept



