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

**Search for a Range** [https://leetcode.com/problems/search-for-a-range/description/](https://leetcode.com/problems/search-for-a-range/description/)

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

## **Search Insert Position**

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input:
 [1,3,5,6], 5

Output:
 2
```

**Example 2:**

```
Input:
 [1,3,5,6], 2

Output:
 1
```

**Example 3:**

```
Input:
 [1,3,5,6], 7

Output:
 4
```

**Example 4:**

```
Input:
 [1,3,5,6], 0

Output:
 0
```

Tips:

all binary search is to find a target

It usually consider find first position or last position of a number

```
class Solution:
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """


        start, end = 0, len(nums) -1
        while start +1 < end:
            mid = start + (end-start)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                start = mid
            elif nums[mid] > target:
                end = mid

        if nums[start] >= target:
            return start
        elif nums[end] >= target:
            return end
        elif nums[end] < target:
            return end +1
```

# Search a 2D Matrix

Write an efficient algorithm that searches for a value in an\_m\_x\_n\_matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```
Input:

matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3

Output:
 true
```

**Example 2:**

```
Input:

matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13

Output:
 false
```

```
class Solution:

    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """

        rows = len(matrix)
        if rows == 0:
            return False
        cols = len(matrix[0])
        if cols == 0:
            return False
        n =  int(rows * cols)
        start, end = 0, n-1
        while start + 1< end:
            mid = start + (end - start)//2
            x, y = divmod(mid, cols)
            if matrix[x][y] == target:
                return True
            elif matrix[x][y] < target:
                start = mid
            elif matrix[x][y] > target:
                end = mid
        x, y = divmod(start, cols)
        if matrix[x][y] == target:
            return True
        x, y = divmod(end, cols)
        if matrix[x][y] == target:
            return True
        return False
```

# Search a 2D Matrix II

Write an efficient algorithm that searches for a value in anmxnmatrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

**Example:**

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.

Given target = `20`, return `false`.

Tips the idea is to first check the left-bottom number

Since 18 is larger than 5 and it's the smallest number of the row, we can ignore the row and move the row above

we keep doing this until 3.3 is less than 5 so we need to check if 5 is on the right side of this row. we move right

Then we find 6 &gt;5 move above. Then we find 5

If there are duplicated numbers, when we found 5, we need to move to right+1 and row -1

```
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """

        rows = len(matrix)
        if rows == 0:
            return False
        cols = len(matrix[0])
        if cols == 0:
            return False
        x, y = rows-1, 0

        while x >= 0 and y < cols:
            cur = matrix[x][y]
            if cur > target:
                x -=1
            elif cur < target:
                y +=1
            elif cur == target:
                return True
```

# First Bad Version

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have`n`versions`[1, 2, ..., n]`and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API`bool isBadVersion(version)`which will return whether`version`is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example:**

```py
Given n = 5, and version = 4 is the first bad version.


call isBadVersion(3) -
># The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        start, end = 1, n
        while start +1 <end:
            mid = start + (end - start)//2
            if isBadVersion(mid):
                end = mid
            else:
                start = mid

        if isBadVersion(start):
            return start
        elif isBadVersion(end):
            return end


 false
call isBadVersion(5) -
>
 true
call isBadVersion(4) -
>
 true

Then 4 is the first bad version.
```

```
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        start, end = 1, n
        while start +1 <end:
            mid = start + (end - start)//2
            if isBadVersion(mid):
                end = mid
            else:
                start = mid

        if isBadVersion(start):
            return start
        elif isBadVersion(end):
            return end
```

# Find peak element

# 

O\(logn\) &lt; O\(n\)

O\(Logn\): binary search

A peak element is an element that is greater than its neighbors.

Given an input array`nums`, where`nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that`nums[-1] = nums[n] = -∞`.

**Example 1:**

```
Input:
nums
 = 
[1,2,3,1]
Output:
 2

Explanation:
 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input:
nums
 = 
[
1,2,1,3,5,6,4]

Output:
 1 or 5 

Explanation:
 Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

```
class Solution:
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        start = 0
        end = len(nums) - 1
        while start +1 < end:
            mid = start + (end - start)//2
            if nums[mid] < nums[mid+1]:
                start = mid
            elif nums[mid] > nums[mid+1]:
                end = mid


        if nums[start] > nums[end]:
            return start
        else:
            return end
```

---

# Rotated  Array

it's similar to the binary search

e,g, \(i.e.,

`[0,1,2,4,5,6,7]`

might become

`[4,5,6,7,0,1,2]`

\).

If the array is just an increasing array, the start may not exist. but the end is always exist

```
        start = 0
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)//2
            if nums[mid] >= nums[end]:
                start = mid
            else:
                end = mid
```

# Find Minimum in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`\).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**

```
Input:
 [3,4,5,1,2] 

Output:
 1
```

**Example 2:**

```
Input:
 [4,5,6,7,0,1,2]

Output:
 0
```

```
class Solution:
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int

        """
        start = 0
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)//2
            if nums[mid] >= nums[end]:
                start = mid
            else:
                end = mid

        return min(nums[start],nums[end])
```

**If the array has duplicated number**



**black box test**

**If the duplicated number exist, following the previous example, if we have an array with all value to be 1 and only one value is 0**

**so we have to look all element to check the value. The time complex must be N**



## Find Minimum in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`\).

Find the minimum element.

The array may contain duplicates.

**Example 1:**

```
Input:
 [1,3,5]

Output:
 1
```

**Example 2:**

```
Input:
 [2,2,2,0,1]

Output:
 0
```

**Note:**

* This is a follow up problem to 
  [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)
  .
* Would allow duplicates affect the run-time complexity? How and why?

```
class Solution:
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return min(nums)
        
```



