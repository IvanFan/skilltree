# Python Basic

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

Usually we don't use it. One example use case is Django ORM

It allows you to define something like this:

```py
class Person(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()
```

But if you do this:

```
guy = Person(name='bob', age='35')
print(guy.age)
```

It won't return an IntegerField object. It will return an int, and can even take it directly from the database.

This is possible because models.Model defines \_\_metaclass\_\_ and it uses some magic that will turn the Person you just defined with simple statements into a complex hook to a database field.

Django makes something complex look simple by exposing a simple API and using metaclasses, recreating code from this API to do the real job behind the scenes.

First, you know that classes are objects that can create instances.

**Well in fact, classes are themselves instances. Of metaclasses.**

Everything is an object in Python, and they are all either instances of classes or instances of metaclasses.

Except for **type. Type ** is actually its own metaclass. This is not something you could reproduce in pure Python, and is done by cheating a little bit at the implementation level.

Secondly, metaclasses are complicated. You may not want to use them for very simple class alterations. You can change classes by using two different techniques:

* [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch)
* class decorators

## @staticmethod和@classmethod

There are 3 methods in python. staticmethod classmethod and instance method:

```py
def foo(x):
    print "executing foo(%s)"%(x)

class A(object):
    def foo(self,x):
        print "executing foo(%s,%s)"%(self,x)

    @classmethod
    def class_foo(cls,x):
        print "executing class_foo(%s,%s)"%(cls,x)

    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)"%x

a=A()
```

self & cls:

self and cls is a binding of instance and class. Normally, we can call foo\(x\) which has nothing to do with class or instance.

if we use foo\(self,x\), we will pass the instance it self into the function

It's similar to cls. However, if we use static method then we don't need to pass the self or cls

|  |
| :--- |


|  | instance method | class method | static method |
| :--- | :--- | :--- | :--- |
| a = A\(\) | a.foo\(x\) | a.class\_foo\(x\) | a.static\_foo\(x\) |
| A | 不可用 | A.class\_foo\(x\) | A.static\_foo\(x\) |

## Class variable & instance variable

class variable can be used between different class instance

instance variable can only be used for this instance

```py
class Test(object):  
    num_of_instance = 0  
    def __init__(self, name):  
        self.name = name  
        Test.num_of_instance += 1  

if __name__ == '__main__':  
    print Test.num_of_instance   # 0
    t1 = Test('jack')  
    print Test.num_of_instance   # 1
    t2 = Test('lucy')  
    print t1.name , t1.num_of_instance  # jack 2
    print t2.name , t2.num_of_instance  # lucy 2
```

```py
class Person:
    name="aaa"

p1=Person()
p2=Person()
p1.name="bbb"
print p1.name  # bbb
print p2.name  # aaa
print Person.name  # aaa
```

here p1.name= "bbb". At first, p1.name is refereced to Class variable name. But after assigning the value, p1.name is referenced to a new string

```py
class Person:
    name=[]

p1=Person()
p2=Person()
p1.name.append(1)
print p1.name  # [1]
print p2.name  # [1]
print Person.name  # [1]
```

## Python introspection

It means getting the type of the object during the runtime

e.g. type\(\) dir\(\) getattr\(\) hasattr\(\) isinstance\(\)

```py
a = [1,2,3]
b = {'a':1,'b':2,'c':3}
c = True
print(type(a),type(b),type(c)) # <type 'list'> <type 'dict'> <type 'bool'>
print(isinstance(a,list))  # True
```

## Dict items

```py
dict_item = { 'a':1, 'b':2, 'c':3  }
d = {key: value for (key, value) in dict_item.items()}
print(d)
```

## \_ and \_\_ in Python

```py
>>> class MyClass():
...     def __init__(self):
...             self.__superprivate = "Hello"
...             self._semiprivate = ", world!"
...
>>> mc = MyClass()
>>> print mc.__superprivate
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: myClass instance has no attribute '__superprivate'
>>> print mc._semiprivate
, world!
>>> print mc.__dict__
{'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
```

\_\__init\_\_\_: to represent native functions

`__init__()`

`__del__()`

`__call__()`

\_foo: it means the variable is private. The private variable cannot be imported.

\_\_foo: complier will use **\_classname\_\_foo**_ to replace the original name. It cannot be used like public variables. We can call it via \_instanceName.\*\*\_classname\_\_foo\*\*

## 

## String format: % and .format

there is a trick about %

```
print("hi there %s" % name)
```

it looks all good. But if name=\(1,2,3\)

it will throw type error. In this case we have to use

```
"hi there %s" % (name,)
```

if you want to use py2.5 then you may have to use % instead of .format

## Iterables & Generator

**iterables**

if you create a list which can be read one by one, this is called iteration

```
>>> mylist = [1, 2, 3]
>>> for i in mylist:
...    print(i)
1
2
3
```

```
>>> mylist = [x*x for x in range(3)]
>>> for i in mylist:
...    print(i)
0
1
4
```

it's easy to use and read. But you have to save the values into memory.

**Generators**

generator is a kind of iteration. However, generate can only be looped once. Because it's now all saved in memory.

```
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator:
...    print(i)
0
1
4
```

The difference compared with iteration is \(\). Besides you cannot use for i in mygenerator twice

**Yield**

it's similar to return

```
>>> def createGenerator():
...    mylist = range(3)
...    for i in mylist:
...        yield i*i
...
>>> mygenerator = createGenerator() # 创建生成器
>>> print(mygenerator) # mygenerator is an object!
<generator object createGenerator at 0xb7555c34>
>>> for i in mygenerator:
...     print(i)
0
1
4
```

If you have a huge list and you only want to read it once, then yield is the best choice

When you calling the function, the code within the function is actually not running.** It just return the generator object**

When we using for to go through the iteration, the code will work.

The difficult part is:

When the for loop print the returned generator object for the first time, the code in the function begin to work until it reach the keyword yield. Then it returns the first response. Next loop will also run only once and return one response

let see another example:

We can create a generator first

```
# 这里你创建node方法的对象将会返回一个生成器
def node._get_child_candidates(self, distance, min_dist, max_dist):

  # 这里的代码你每次使用生成器对象的时候将会调用

  if self._leftchild and distance - max_dist < self._median:
      yield self._leftchild

  if self._rightchild and distance + max_dist >= self._median:
      yield self._rightchild

  # 如果代码运行到这里,生成器就被认为变成了空的
```

Call the function:

```
# 创建空列表和一个当前对象索引的列表
result, candidates = list(), [self]

# 在candidates上进行循环(在开始只保含一个元素)
while candidates:

    # 获得最后一个condidate然后从列表里删除
    node = candidates.pop()

    # 获取obj和candidate的distance
    distance = node._get_dist(obj)

    # 如果distance何时将会填入result
    if distance <= max_dist and distance >= min_dist:
        result.extend(node._values)

    candidates.extend(node._get_child_candidates(distance, min_dist, max_dist))

return result
```

extend\(\) is a list object method

```
>>> a = [1, 2]
>>> b = [3, 4]
>>> a.extend(b)
>>> print(a)
[1, 2, 3, 4]
```

another example

```
>>> class Bank(): # 让我们建个银行,生产许多ATM
...    crisis = False
...    def create_atm(self):
...        while not self.crisis:
...            yield "$100"
>>> hsbc = Bank() # 当一切就绪了你想要多少ATM就给你多少
>>> corner_street_atm = hsbc.create_atm()
>>> print(corner_street_atm.next())
$100
>>> print(corner_street_atm.next())
$100
>>> print([corner_street_atm.next() for cash in range(5)])
['$100', '$100', '$100', '$100', '$100']
>>> hsbc.crisis = True # cao,经济危机来了没有钱了!
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> wall_street_atm = hsbc.create_atm() # 对于其他ATM,它还是True
>>> print(wall_street_atm.next())
<type 'exceptions.StopIteration'>
>>> hsbc.crisis = False # 麻烦的是,尽管危机过去了,ATM还是空的
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> brand_new_atm = hsbc.create_atm() # 只能重新新建一个bank了
>>> for cash in brand_new_atm:
...    print cash
$100
$100
$100
$100
$100
$100
$100
$100
$100
...
```

**Itertools**

```
>>> horses = [1, 2, 3, 4]
>>> races = itertools.permutations(horses)
>>> print(races)
<itertools.permutations object at 0xb754f1dc>
>>> print(list(itertools.permutations(horses)))
[(1, 2, 3, 4),
 (1, 2, 4, 3),
 (1, 3, 2, 4),
 (1, 3, 4, 2),
 (1, 4, 2, 3),
 (1, 4, 3, 2),
 (2, 1, 3, 4),
 (2, 1, 4, 3),
 (2, 3, 1, 4),
 (2, 3, 4, 1),
 (2, 4, 1, 3),
 (2, 4, 3, 1),
 (3, 1, 2, 4),
 (3, 1, 4, 2),
 (3, 2, 1, 4),
 (3, 2, 4, 1),
 (3, 4, 1, 2),
 (3, 4, 2, 1),
 (4, 1, 2, 3),
 (4, 1, 3, 2),
 (4, 2, 1, 3),
 (4, 2, 3, 1),
 (4, 3, 1, 2),
 (4, 3, 2, 1)]
```

Interview question: if we change the \[\]  into \(\) , will the data structure be changed

Answer: yes i's changed from list to generator

```py
>>> L = [x*x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x*x for x in range(10))
>>> g
<generator object <genexpr> at 0x0000028F8B774200>
```

## \*args and \*\*kwargs

If you don't know how many parameters will be passed into function, you can use \*args

```
>>> def print_everything(*args):
        for count, thing in enumerate(args):
...         print '{0}. {1}'.format(count, thing)
...
>>> print_everything('apple', 'banana', 'cabbage')
0. apple
1. banana
2. cabbage
```

similar, \*\*kwargs allows you to use parameters which haven't been declared.

```
>>> def table_things(**kwargs):
...     for name, value in kwargs.items():
...         print '{0} = {1}'.format(name, value)
...
>>> table_things(apple = 'fruit', cabbage = 'vegetable')
cabbage = vegetable
apple = fruit
```

We also can use named parameter and \*args \*\*kwargs together

```
def table_things(titlestring, **kwargs)
```

You can also use \* or \*\* for functions

```
>>> def print_three_things(a, b, c):
...     print 'a = {0}, b = {1}, c = {2}'.format(a,b,c)
...
>>> mylist = ['aardvark', 'baboon', 'cat']
>>> print_three_things(*mylist)

a = aardvark, b = baboon, c = cat
```

## AOP & decorators

AOP is a programming concept. E.g. Spring framework has used AOP for syntax suggers

It can be used for logging, performance testing ...

In python, we can use decorator to implement AOP. **Decorator adds extra functions to the existing objects.**

First all functions in python are obejcts:

```
def shout(word="yes"):
    return word.capitalize()+"!"

print shout()
# 输出 : 'Yes!'

# 作为一个对象,你可以把它赋值给任何变量

scream = shout

# 注意啦我们没有加括号,我们并不是调用这个函数,我们只是把函数"shout"放在了变量"scream"里.
# 也就是说我们可以通过"scream"调用"shout":

print scream()
# 输出 : 'Yes!'

# 你可以删除旧名"shout",而且"scream"依然指向函数

del shout
try:
    print shout()
except NameError, e:
    print e
    #输出: "name 'shout' is not defined"

print scream()
# 输出: 'Yes!'
```

Second, you can define another function in a function

```
def talk():

    # 你可以在"talk"里定义另一个函数 ...
    def whisper(word="yes"):
        return word.lower()+"..."

    # 让我们用用它!

    print whisper()

# 每次调用"talk"时都会定义一次"whisper",然后"talk"会调用"whisper"
talk()
# 输出:
# "yes..."

# 但是在"talk"意外"whisper"是不存在的:

try:
    print whisper()
except NameError, e:
    print e
    #输出 : "name 'whisper' is not defined"*
```

Third, the function can return another function

```
def getTalk(kind="shout"):

    # 在函数里定义一个函数
    def shout(word="yes"):
        return word.capitalize()+"!"

    def whisper(word="yes") :
        return word.lower()+"...";

    # 返回一个函数
    if kind == "shout":
        # 这里不用"()",我们不是要调用函数
        # 只是返回函数对象
        return shout
    else:
        return whisper

# 怎么用这个特性呢?

# 把函数赋值给变量
talk = getTalk()

# 可以看到"talk"是一个函数对象
print talk
# 输出 : <function shout at 0xb7ea817c>

# 函数返回的是对象:
print talk()
# 输出 : Yes!

# 不嫌麻烦你也可以这么用
print getTalk("whisper")()
# 输出 : yes...
```

Last, you can also use function as parameter

```
def doSomethingBefore(func):
    print "I do something before then I call the function you gave me"
    print func()

doSomethingBefore(scream)
# 输出:
#I do something before then I call the function you gave me
#Yes!
```

example:

I want to get

```
<b><i>Hello</i></b>
```

```py
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
```

how to create decator

```
# 装饰器就是把其他函数作为参数的函数
def my_shiny_new_decorator(a_function_to_decorate):

    # 在函数里面,装饰器在运行中定义函数: 包装.
    # 这个函数将被包装在原始函数的外面,所以可以在原始函数之前和之后执行其他代码..
    def the_wrapper_around_the_original_function():

        # 把要在原始函数被调用前的代码放在这里
        print "Before the function runs"

        # 调用原始函数(用括号)
        a_function_to_decorate()

        # 把要在原始函数调用后的代码放在这里
        print "After the function runs"

    # 在这里"a_function_to_decorate" 函数永远不会被执行
    # 在这里返回刚才包装过的函数
    # 在包装函数里包含要在原始函数前后执行的代码.
    return the_wrapper_around_the_original_function

# 加入你建了个函数,不想修改了
def a_stand_alone_function():
    print "I am a stand alone function, don't you dare modify me"

a_stand_alone_function()
#输出: I am a stand alone function, don't you dare modify me

# 现在,你可以装饰它来增加它的功能
# 把它传递给装饰器,它就会返回一个被包装过的函数.

a_stand_alone_function_decorated = my_shiny_new_decorator(a_stand_alone_function)
a_stand_alone_function_decorated()
#输出s:
#Before the function runs
#I am a stand alone function, don't you dare modify me
#After the function runs
```

syntax

```
@my_shiny_new_decorator
def another_stand_alone_function():
    print "Leave me alone"

another_stand_alone_function()
#输出:
#Before the function runs
#Leave me alone
#After the function runs
```

is equal to

```
another_stand_alone_function = my_shiny_new_decorator(another_stand_alone_function)
```

**Advance decorator **

we can pass parameters into the decorator

```
# 这不是什么黑魔法,你只需要让包装器传递参数:

def a_decorator_passing_arguments(function_to_decorate):
    def a_wrapper_accepting_arguments(arg1, arg2):
        print "I got args! Look:", arg1, arg2
        function_to_decorate(arg1, arg2)
    return a_wrapper_accepting_arguments

# 当你调用装饰器返回的函数时,也就调用了包装器,把参数传入包装器里,
# 它将把参数传递给被装饰的函数里.

@a_decorator_passing_arguments
def print_full_name(first_name, last_name):
    print "My name is", first_name, last_name

print_full_name("Peter", "Venkman")
# 输出:
#I got args! Look: Peter Venkman
#My name is Peter Venkman
```

more complex

```py
def decorator_with_args(decorator_to_enhance):
    """
    这个函数将被用来作为装饰器.
    它必须去装饰要成为装饰器的函数.
    休息一下.
    它将允许所有的装饰器可以接收任意数量的参数,所以以后你不必为每次都要做这个头疼了.
    saving you the headache to remember how to do that every time.
    """

    # 我们用传递参数的同样技巧.
    def decorator_maker(*args, **kwargs):

        # 我们动态的建立一个只接收一个函数的装饰器,
        # 但是他能接收来自maker的参数
        def decorator_wrapper(func):

            # 最后我们返回原始的装饰器,毕竟它只是'平常'的函数
            # 唯一的陷阱:装饰器必须有这个特殊的,否则将不会奏效.
            return decorator_to_enhance(func, *args, **kwargs)

        return decorator_wrapper
```

```py
# 下面的函数是你建来当装饰器用的,然后把装饰器加到上面:-)
# 不要忘了这个 "decorator(func, *args, **kwargs)"
@decorator_with_args
def decorated_decorator(func, *args, **kwargs):
    def wrapper(function_arg1, function_arg2):
        print "Decorated with", args, kwargs
        return func(function_arg1, function_arg2)
    return wrapper

# 现在你用你自己的装饰装饰器来装饰你的函数(汗~~~)

@decorated_decorator(42, 404, 1024)
def decorated_function(function_arg1, function_arg2):
    print "Hello", function_arg1, function_arg2

decorated_function("Universe and", "everything")
#输出:
#Decorated with (42, 404, 1024) {}
#Hello Universe and everything

# Whoooot!
```



