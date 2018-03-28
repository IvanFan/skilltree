**Assume**:

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order ofO\(logn\).

If the target is not found in the array, return`[-1, -1]`.

For example,  
Given`[5, 7, 7, 8, 8, 10]`and target value 8,  
return`[3, 4]`.

**The idea is first use binary search to find the left side of the target and then use the same method to find the right side.**

To find the left side:

mid = int\(l+r\)/2 // this will always get the low integer e.g. int\(1.6\) == 1

Since it's an ascending order, if nums\[mid\] &lt; target,  then target exist in \(mid, r\], we should have l=mid +1

If nums\[mid\] &gt; target, the target exist in \[l, mid\) so we should have r = mid -1

if nums\[mid\] == target, we have 2 situation:

1. target start == mid
2. target start &lt; mid

Therefore target start &lt;= mid, so target start is within \[l, mid\], we should have r = mid

1 2 3 3 3 4 5

l                   r

1 2 3 3 3 4 5    mid = 3 nums\[3\] == 3

l                   r

1 2 3 3 3 4 5    mid = 1 nums\[1\] == 2 &lt;3

l         r

1 2 3 3 3 4 5    mid = 1 nums\[1\] == 2 &lt;3

l  r

1 2 3 3 3 4 5    mid = 2 nums\[2\] == 3

```
  lr
```

l=r and nums\[l\] is the target left side

Now we have target left side == l. The next step is to find the right side

The right side is within \[l, len\(nums\)-1\]

l = l

r = len\(nums\)-1

mid = int\(l+r\)/2

if nums\[mid\] &gt; target, then the target end should within the \[l, mid\] therefore r = mid -1

if nums\[mid\] == target, then 2 situation

1. target end is on the right side of mid, then l = mid +1

2. target end is mid, l = mid

Therefore target end &gt;= mid, target end is within \[mid r\], then l = mid





