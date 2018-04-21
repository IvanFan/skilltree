## lambda

In general, lambda is an anonymous function

```
map( lambda x: x*x, [y for y in range(10)] )
map( lambda x : x + 1, [1, 2, 3] )

>>> list1 = [3,5,-4,-1,0,-2,-6]
>>> sorted(list1, key=lambda x: abs(x))
[0, -1, -2, 3, -4, 5, -6]
```

## Functional Programming

filter

```
>>>a = [1,2,3,4,5,6,7]
>>>b = filter(lambda x: x > 5, a)
>>>print b
>>>[6,7]
```

map

```
>>> a = map(lambda x:x*2,[1,2,3])
>>> list(a)
[2, 4, 6]
```

reduce get 3!

```
>>> reduce(lambda x,y:x*y,range(1,4))
6
```

The main idea of functional programming is trying to avoid modifying state outside the function.

Same input should always get the same output



