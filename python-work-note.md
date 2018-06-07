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



