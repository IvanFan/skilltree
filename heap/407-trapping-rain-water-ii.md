![](/assets/Screen Shot 2018-06-15 at 12.48.00 am.png)



```py
from heapq import *
class Solution:
    def dfs(self, h, x, y, bd, heightMap, m, n, res):
        for dx, dy in zip([-1 ,1, 0, 0],[0,0,-1,1]):
            if x + dx > 0 and x + dx < m-1 and y +dy >0  and y + dy < n-1 and heightMap[x + dx][y +dy] >=0:
                curr_h = heightMap[x + dx][y +dy]
                heightMap[x + dx][y +dy] = -1
                if curr_h > h:
                    heappush(bd, [curr_h, x + dx, y +dy])
                else:
                    res += h - curr_h
                    res = self.dfs(h, x + dx, y +dy,bd,  heightMap, m, n, res)
        return res
                    
                
    def trapRainWater(self, heightMap):
        """
        :type heightMap: List[List[int]]
        :rtype: int
        """
        if len(heightMap) < 3 or len(heightMap[0]) < 3:
            return 0
        m, n, bd = len(heightMap), len(heightMap[0]), []
        for x, y in zip( [0]*n + [m-1]*n + list(range(1, m-1))*2, list(range(n))*2 + [0]*(m-2) + [n-1]*(m-2)):
            heappush(bd, [heightMap[x][y], x ,y])
            heightMap[x][y] = -1
        res = 0
        while bd:
            h,x,y = heappop(bd)
            res = self.dfs(h, x, y, bd, heightMap, m, n, res) 
        return res
        
```



