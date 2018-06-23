# Binary search

T\(n\) = T\(n/2\) + T\(1\)

O\(LogN\)

Recursion or While-loop?

recursion costs stack space

all ok

if we can use while instead of recursion, then use while

KeyPoints:

the end condition of while:  while start +1 &lt; end \(next to\)

```
while start +1 < end: # start and end do not have to +1 but we need to do extra check for the last two numbers
   mid = start + (end - start)// 2 # 防止溢出 if end is max integer
   if a[mid] ==target:
        end = mid
  elif a[mid] < target:
        start = mid
  elif a[mid] > target:
        end = mid


if a[start] == target:
   return start
if a[end] == target:
   return end
```



