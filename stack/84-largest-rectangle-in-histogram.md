![](/assets/Screen Shot 2018-06-14 at 3.10.21 pm.png)



```py
class Solution:
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        stack = []
        stack.append(-1)
        maxarea = 0
        if len(heights) ==1:
            return heights[0]
        for i in range(len(heights)):
            while len(stack) != 1 and (heights[i] <= heights[stack[-1]]):
                top = stack.pop()
                distance = i - stack[-1] - 1
                height = heights[top]
                area = height * distance
                if maxarea < area:
                    maxarea = area
            stack.append(i)
        while len(stack) != 1:
            top = stack.pop()
            distance = len(heights) - stack[-1] - 1
            height = heights[top]
            area = height * distance
            if maxarea < area:
                maxarea = area
        return maxarea
                
                    
        
```



