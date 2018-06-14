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



