# Two Sum

# 

Find two numbers such that they add up to a specific target number.

This question can be solved from different aspects.

## Two points

Given an array of integers that is already_**sorted in ascending order**_, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

* Your returned answers \(both index1 and index2\) are not zero-based.
* You may assume that each input would have
  _exactly_
  one solution and you may not use the
  _same_
  element twice.

**Example:**

```
Input:
 numbers = [2,7,11,15], target = 9

Output:
 [1,2]

Explanation:
 The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```



```py
class Solution:
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        if len(numbers) < 2:
            return []
        l = 0
        r = len(numbers)-1
        while l < r:
            sumnum = numbers[l] + numbers[r]
            if sumnum == target:
                return [l+1,r+1]
            elif sumnum > target:
                r -=1
            else:
                l += 1
            
           
        return []
                
            
            
        
```


