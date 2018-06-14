![](/assets/Screen Shot 2018-06-14 at 11.12.56 pm.png)

```py
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e
import collections
import heapq

class Solution:
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: int
        """
        points = collections.defaultdict(list)
        for input in intervals:
            points[input.start].append(input)
            points[input.end].append(input)

        current_meeting = []
        max_meetings = 0
        for point in sorted(list(points.keys()), key=lambda x:x):
            meetings = points[point]
            for meeting in meetings:
                if point == meeting.start:
                    heapq.heappush(current_meeting, meeting.start)
                elif point == meeting.end:
                    current_meeting.remove(meeting.start)
                    # heapq.heapify(current_meeting)
            max_meetings = max(max_meetings, len(current_meeting))
        return max_meetings
```



Same idea of the previous question

