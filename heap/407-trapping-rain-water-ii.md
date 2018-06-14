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

这道题是之前那道[Trapping Rain Water](http://www.cnblogs.com/grandyang/p/4402392.html)的拓展，由2D变3D了，感觉很叼。但其实解法跟之前的完全不同了，之前那道题由于是二维的，我们可以用双指针来做，而这道三维的，我们需要用BFS来做，解法思路很巧妙，下面我们就以题目中的例子来进行分析讲解，多图预警，手机流量党慎入：

首先我们应该能分析出，能装水的底面肯定不能在边界上，因为边界上的点无法封闭，那么所有边界上的点都可以加入queue，当作BFS的启动点，同时我们需要一个二维数组来标记访问过的点，访问过的点我们用红色来表示，那么如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003120051785-2137527353.jpg)

我们再想想，怎么样可以成功的装进去水呢，是不是周围的高度都应该比当前的高度高，形成一个凹槽才能装水，而且装水量取决于周围最小的那个高度，有点像木桶原理的感觉，那么为了模拟这种方法，我们采用模拟海平面上升的方法来做，我们维护一个海平面高度mx，初始化为最小值，从1开始往上升，那么我们BFS遍历的时候就需要从高度最小的格子开始遍历，那么我们的queue就不能使用普通队列了，而是使用优先级队列，将高度小的放在队首，最先取出，这样我们就可以遍历高度为1的三个格子，用绿色标记出来了，如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003120900692-314831597.jpg)

如上图所示，向周围BFS搜索的条件是不能越界，且周围格子未被访问，那么可以看出上面的第一个和最后一个绿格子无法进一步搜索，只有第一行中间那个绿格子可以搜索，其周围有一个灰格子未被访问过，将其加入优先队列queue中，然后标记为红色，如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003121601785-791353712.jpg)

那么优先队列queue中高度为1的格子遍历完了，此时海平面上升1，变为2，此时我们遍历优先队列queue中高度为2的格子，有3个，如下图绿色标记所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003121753379-2129390635.jpg)

我们发现这三个绿格子周围的格子均已被访问过了，所以不做任何操作，海平面继续上升，变为4，遍历所有高度为4的格子，如下图绿色标记所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003121905207-4147516.jpg)

由于我们没有特别声明高度相同的格子在优先队列queue中的顺序，所以应该是随机的，其实谁先遍历到都一样，对结果没啥影响，我们就假设第一行的两个绿格子先遍历到，那么那么周围各有一个灰格子可以遍历，这两个灰格子比海平面低了，可以存水了，把存水量算出来加入结果res中，如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003122502614-299103818.jpg)

上图中这两个遍历到的蓝格子会被加入优先队列queue中，由于它们的高度小，所以下一次从优先队列queue中取格子时，它们会被优先遍历到，那么左边的那个蓝格子进行BFS搜索，就会遍历到其左边的那个灰格子，由于其高度小于海平面，也可以存水，将存水量算出来加入结果res中，如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003122735129-1777610725.jpg)

等两个绿格子遍历结束了，它们会被标记为红色，蓝格子遍历会先被标记红色，然后加入优先队列queue中，由于其周围格子全变成红色了，所有不会有任何操作，如下图所示：

![](https://images2015.cnblogs.com/blog/391947/201610/391947-20161003122909770-1492433975.jpg)

此时所有的格子都标记为红色了，海平面继续上升，继续遍历完优先队列queue中的格子，不过已经不会对结果有任何影响了，因为所有的格子都已经访问过了，此时等循环结束后返回res即可

