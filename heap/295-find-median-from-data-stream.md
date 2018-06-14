![](/assets/Screen Shot 2018-06-14 at 11.15.39 pm.png)

```py
import heapq 

class MedianFinder:
    small_half_max_top = None
    large_half_min_top = None
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.small_half_max_top = []
        self.large_half_min_top = []



    def addNum(self, num):
        """
        :type num: int
        :rtype: void
        """
        heapq.heappush(self.small_half_max_top, -1 * num)
        heapq.heappush(self.large_half_min_top,  heapq.heappop(self.small_half_max_top)* -1)
        if len(self.small_half_max_top)  < len(self.large_half_min_top):
            heapq.heappush(self.small_half_max_top,  heapq.heappop(self.large_half_min_top)* -1)

        # print(self.small_half_max_top)
        # print(self.large_half_min_top)


    def findMedian(self):
        """
        :rtype: float
        """
        total_len = len(self.small_half_max_top) + len(self.large_half_min_top) 
        if total_len == 0:
            return 0
        if total_len % 2 == 0:
            num1 = self.large_half_min_top[0]
            num2 = self.small_half_max_top[0]
            return (num1 + num2* -1)/2
        else:
            num2 = self.small_half_max_top[0]
            return float(num2 * -1)
```

```
small heap: [1]
large heap: []

small heap: []
large heap: [1]

small heap: [1]
large heap: []

small heap: [1 2]
large heap: []

small heap: [1]
large heap: [2]

small heap: [1 3]
large heap: [2]

small heap: [1]
large heap: [2 3]

small heap: [1 2]
large heap: [3]




```







The idea of this question is to hold two heaps:

a heap contains all elements from smallest to mid

a heap contains all elements from largest to mid

Therefore, if a new element come in, we first push it into small part

Then get the largest element from small part and push it into larger part

At the same time we need to keep the length of small part &gt;= larger part and the max difference is 1

So if length of small part &lt; larger part, we need to get the smallest element from larger part and push it into the smallest part







