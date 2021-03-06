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

3 3 3 4 5  mid = 2 nums\[2\] = 3

l             r

3 3 3 4 5  mid = 3 nums\[3\] &gt; 3

l      r

3 3 3 4 5 mid = 2  nums\[2\] = 3 l=

```
   l  r
```

Then we have the problem that at some stage we alway have l = min

So l pointer stops moving.

To avoid this we can let mid = l+r+1/2, So everytime we get the highest integer as mid

3 3 3 4 5  mid = 2 nums\[2\] = 3

l             r

3 3 3 4 5  mid = 3 nums\[3\] &gt; 3

```
   l      r
```

3 3 3 4 5 mid = 3  nums\[3\] =4 &gt; 3

```
   l  r
```

3 3 3 4 5

```
   l=r=2
```



#### Search in Rotated Sorted Array II  

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).

Write a function to determine if a given target is in the array.

The array may contain duplicates.



```
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        nl = len(nums)
        if nl == 0:
            return False
        if nl == 1:
            return nums[0] == target
        l = 0
        r = nl - 1
        while l <= r:
            print('l:',l,'r:',r)
            mid = int((l+r)/2)
            if nums[mid]==target:
                return True
            while nums[l] == nums[mid] and l < mid:
                l += 1
            print('sec l:',l,'r:',r)
            if nums[l] <= nums[mid]:
                if nums[l] <= target < nums[mid]:
                    r = mid -1
                else:
                    l = mid +1
            else:
                if nums[mid] < target <= nums[r]:
                    l = mid + 1
                else:
                    r = mid -1
       
        return False
        
                    
            
            
            
        
```

  


