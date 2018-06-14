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



