![](/assets/Screen Shot 2018-06-15 at 12.42.01 am.png)



```py
import heapq

class Solution:
    def nthUglyNumber(self, n):
        """
        :type n: int
        :rtype: int
        """
        q = []
        heapq.heappush(q, 1)
        ans = 0
        while n > 0:
            while q and q[0] <= ans:
                heapq.heappop(q)
            ans = heapq.heappop(q)
            n -= 1
            heapq.heappush(q, ans * 2)
            heapq.heappush(q, ans * 3)
            heapq.heappush(q, ans * 5)
        return ans
        
        
        
        
```



