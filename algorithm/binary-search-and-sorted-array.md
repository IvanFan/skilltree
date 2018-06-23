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

e.g. Search for a Range https://leetcode.com/problems/search-for-a-range/description/

Given an array of integers`nums`sorted in ascending order, find the starting and ending position of a given`target`value.

Your algorithm's runtime complexity must be in the order of_O_\(log_n_\).

If the target is not found in the array, return`[-1, -1]`.

**Example 1:**

```
Input:
 nums = [
5,7,7,8,8,10]
, target = 8

Output:
 [3,4]
```

**Example 2:**

```
Input:
 nums = [
5,7,7,8,8,10]
, target = 6

Output:
 [-1,-1]
```

---

tips:Binary search can find the target, start of the range and end of the range

So we only need to do it twice.

```py
class Solution:
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return [-1, -1]
        l = 0
        r = len(nums) - 1

        res= []
        left = -1
        right = -1
        # find the start 
        while l +1 < r:
            mid = l+ (r-l)//2
            if nums[mid] == target:
                r = mid
            elif nums[mid] < target:
                l = mid
            elif nums[mid] > target:
                r = mid
        if nums[l] == target:
            left = l
        elif nums[r] == target:
            left = r
            
        l = 0
        r = len(nums) - 1
        
        # find the end 
        while l +1 < r:
            mid = l+ (r-l)//2
            if nums[mid] == target:
                l = mid
            elif nums[mid] < target:
                l = mid
            elif nums[mid] > target:
                r = mid
                
        if nums[r] == target:
            right = r
        elif nums[l] == target:
            right = l
            
        return [left, right]
        

```



