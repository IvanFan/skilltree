# Sorted Array

## Merge Sorted Array

![](/assets/Screen Shot 2018-06-28 at 10.07.25 pm.png)

Tips:

create a new array

check the top of each array and get the smallest element

## Merge Sorted Array \|\|

```
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:

The number of elements initialized in nums1 and nums2 are m and n respectively.
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.
```

**Example:**

```
Input:

nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3


Output:
 [1,2,2,3,5,6]
```

```
class Solution:
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: void Do not return anything, modify nums1 in-place instead.
        """
        s1 = m -1
        s2 = n -1
        for i in range(len(nums1)-1,-1, -1):
            if s1 >= 0 and s2 >=0:
                if nums1[s1] >= nums2[s2]:
                    nums1[i] = nums1[s1]
                    s1 -=1
                else:
                    nums1[i] = nums2[s2]
                    s2 -=1
            elif s1 >= 0:
                nums1[i] = nums1[s1] 
                s1 -=1
            elif s2 >= 0:
                nums1[i] = nums2[s2]
                s2 -=1
```

The tips is to compared the numbers from the very end

we need to think reversely

---

## Median of Two Sorted Arrays

There are two sorted arrays**nums1**and**nums2**of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O\(log \(m+n\)\).

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

tips:

find kth in the sorted array

median  = find\(\(a +b\) / 2\)

![](/assets/Screen Shot 2018-06-28 at 11.13.53 pm.png)so we can remove the first k/2 from a

so now k - k/2 will be the number we need to find

So we can convert O\(k\) =&gt; O\(k/2\) in O\(1\) operation



