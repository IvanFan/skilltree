![](/assets/Screen Shot 2018-06-14 at 2.25.45 pm.png)

```py
import collections

class Solution(object):
    def removeDuplicateLetters(self, s):
        """
        :type s: str
        :rtype: str
        """
        stack = []
        count = collections.defaultdict(int)
        visit = collections.defaultdict(int)
        for char in s:            
            count[char] += 1
        for char in s:
            count[char] -= 1
            if visit[char] == 0:
                while len(stack) != 0 and stack[-1] > char and count[stack[-1]] > 0:
                    visit[stack[-1]] = 0
                    stack.pop()
                stack.append(char)
                visit[char] += 1
        return ''.join(stack)
```

The idea is the find the smallest letter and put it at the beginning. 

Then the problem is,  how can we make sure that all letter we have met previously will occur again?



so we now convert the problem to:

 we need to check if all previous values can occur again in the future



The idea is to use a hash table to calculate the number of all letters. 

Since we need to use the previous values under certain condition, we can use a stack to hold them



