![](/assets/Screen Shot 2018-06-15 at 12.53.15 am.png)



```py
import collections
import heapq

class Solution:
    def rearrangeString(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: str
        """
        count = collections.defaultdict(int)
        if k == 0:
            return s
        for char in s:
            count[char] +=1 
        hq = []
        res = []
        for key in count.keys():
            heapq.heappush(hq, (-count[key], key))
        while len(res) < len(s):
            stack = []
            for i in range(k):
                if len(hq) == 0:
                    return ''
                prio, val = heapq.heappop(hq)
                res.append(val)
                if len(res) == len(s):
                    return ''.join(res)
                if prio < -1:
                    stack.append((prio+1, val))
            while stack:
                heapq.heappush(hq, stack.pop())
                
        return ''.join(res)

                
        
```



