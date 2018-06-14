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
            while q and q[0] == ans:
                heapq.heappop(q)
            ans = heapq.heappop(q)
            n -= 1
            heapq.heappush(q, ans * 2)
            heapq.heappush(q, ans * 3)
            heapq.heappush(q, ans * 5)
        return ans
```

This is another classic heap problem

First we push the first element into the heap

The get the element from the heap and push all elements relevant to this element

We need to avoid duplicated elements

