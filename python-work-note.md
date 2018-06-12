Slice

```py
customised_code = 'abcdefg'
print("".join(customised_code[:4]))
print("".join(customised_code[4:6]))

list_code = ['a','b', 'c','d','e','f','g']
print("".join(list_code[:4]))
print("".join(list_code[4:6]))
print("".join(list_code[0:4] + list_code[5:]))
```

Stack:

first  come last served

stack.pop\(\) remove last one

stack.top\(\) get last one

result = second\_number / first\_number

if result &gt; 0:

```
result = math.floor\(result\)
```

else:

```
result = math.ceil\(result\)
```

It's equal to int\(\)

if "asdf" not in sthstring:

continue

str.index\(\)

str.isalnum\(\)

str.isalpha\(\)

atr.isdigit\(\)

str.isdecimal\(\)

str. isnumeric\(\)

```
str.isdigit() doesn't work for '-12' negative number
for this issue, it's better to create a separate function

def is_digit(self, n):
        try:
            int(n)
            return True
        except ValueError:
            return  False

for number in string, it's better to consider about multiple situations:
it can be '9' single number but can also be '100' consist of multiple numbers
```

```
from datetime import date, datetime

def json_serial(obj):
    """JSON serializer for objects not serializable by default json code"""

    if isinstance(obj, (datetime, date)):
        return obj.isoformat()
    raise TypeError ("Type %s not serializable" % type(obj))
```

```
# RE parse all the operator and number, put into a list
        # For example, "(2+6* 3+5- (3*14/7+2)*5)+3" gave the following list:
        # ['(', '2', '+', '6', '*', '3', '+', '5', '-', '(', '3', '*', '14', '/', '7', '+', '2', ')', '*', '5', ')', '+', '3']
        for i in re.compile('\d+|[(+-/\*)]').findall(s):

s = "(2+6* 3+5- (3*14/7+2)*5)+3"
print(re.compile('\d+|[(+-/\*)]').findall(s))
['(', '2', '+', '6', '*', '3', '+', '5', '-', '(', '3', '*', '14', '/', '7', '+', '2', ')', '*', '5', ')', '+', '3']
```

```
2, 1 = divmod(5, 2)
```

```
test = -234
abs(test)
234
```

```
The lowest valued entries are retrieved first 
(the lowest valued entry is the one returned by sorted(list(entries))[0]). 
A typical pattern for entries is a tuple in the form: (priority_number, data).

import queue

pq = queue.PriorityQueue()
pq.put((2, [2]))
pq.put((3, [4]))
pq.put((1, [4]))
while not pq.empty():
    key,val =  pq.get()
    print(key, val)
1 [4]
2 [2]
3 [4]
```

```
points = {}
points[2] = 2
points[1] = 1
points[3] = 3
points[4] = 4
print(sorted(list(points.keys()), key=lambda x:x))
[1, 2, 3, 4]
```

```
a = list()
a.append(x)
a.remove(x)
```

```
import heapq
hp = []
heapq.heappush(hp, -b[2])
hp.remove(-b[2])
heapq.heapify(hp)
hp[0]==heapq.heappop(hp)
```

```
test = [[0, 30],[20, 30],[15, 20]]
sorted(test, key=lambda x: x[0])
[[0, 30], [15, 20], [20, 30]]
```

```
print(int('05'))
5
```



