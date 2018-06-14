![](/assets/Screen Shot 2018-06-14 at 10.52.36 pm.png)

```py
import collections
import heapq

class Solution:
    def getSkyline(self, buildings):
        """
        :type buildings: List[List[int]]
        :rtype: List[List[int]]
        """
        # find the points and record building to the point
        points = collections.defaultdict(list)
        end = 0
        for building in buildings:
            points[building[0]].append(building)
            points[building[1]].append(building)

        hp = []
        res = []
        for key in sorted(list(points.keys()), key=lambda x:x):
            bs = points[key]
            for b in bs:
                if key == b[0]:
                    heapq.heappush(hp, -b[2])
                else:
                    hp.remove(-b[2])
                    heapq.heapify(hp)
            if len(hp) == 0:
                res.append((key, 0))
            else:
                if len(res) == 0 or res[-1][1] != abs(hp[0]):
                    res.append((key, abs(hp[0])))

        return res
```



The idea of this question is quite popular

1. Find all cirtical points \(e.g. start and end points\) and put the point and it's relevant events into the hash together

2. go through all cirtical points, for each point, find all relevant events of it

3. if it's the start of the event, push the event into heap. If it's end of the event, remove the event. 

4. after the operations, check how many events are in the heap. Then based on the condition to do some operations

