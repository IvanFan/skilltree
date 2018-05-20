# Two Sum

# 

Find two numbers such that they add up to a specific target number.

It can also be extended to more than 2 numbers

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

From this question, we can see two conditions:

1. order array
2. the number don't have to be the continues

We set the start to be left point and end to be the right point.

Every time we check of left and right number, if larger than target, right point moves left. Otherwise, left point moves right.

## HashTable

Design and implement a TwoSum class. It should support the following operations:`add`and`find`.

`add`- Add the number to an internal data structure.  
`find`- Find if there exists any pair of numbers which sum is equal to the value.

**Example 1:**

```
add(1); add(3); add(5);
find(4) -
>
 true
find(7) -
>
 false
```

**Example 2:**

```
add(3); add(1); add(2);
find(3) -
>
 true
find(6) -
>
 false
```

```py
class TwoSum:

    def __init__(self):
        """
        Initialize your data structure here.
        """

        self.hnums = {}

    def add(self, number):
        """
        Add the number to an internal data structure..
        :type number: int
        :rtype: void
        """
        # try:
        #     val = int(userInput)
        # except ValueError:

        if not self.hnums.get(number):
            self.hnums[number] = 1
        else:
            self.hnums[number] += 1
        #print(self.hnums)

    def find(self, value):
        """
        Find if there exists any pair of numbers which sum is equal to the value.
        :type value: int
        :rtype: bool
        """
        #print('value:',value)
        #print(self.hnums)
        for index, num in enumerate(self.hnums):
            if value - num in self.hnums and (value - num != num or self.hnums[num] > 1):
                #print(value, value - num, index)
                return True
        return False




# Your TwoSum object will be instantiated and called as such:
# obj = TwoSum()
# obj.add(number)
# param_2 = obj.find(value)
```



