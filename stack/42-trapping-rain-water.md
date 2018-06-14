![](/assets/Screen Shot 2018-06-14 at 11.28.41 am.png)

```py
class Solution:
    def trap(self, height):
        ans = 0
        stack = []
        for i in range(len(height)):
            if len(stack) ==0 or height[i] <= height[stack[-1]]:
                stack.append(i)
            else:
                while len(stack) != 0 and height[i] > height[stack[-1]]:
                    top = stack.pop()
                    distance = i - stack[-1] - 1 if len(stack) != 0 else i - 1
                    bound_height = min(height[i], height[stack[-1]]) - height[top] if len(stack) != 0 else 0
                    ans += distance*bound_height
                stack.append(i)
        return ans


    def trapDynamicProgramming(self, height):
        ans = 0
        left_max = [0] * len(height)
        right_max = [0] * len(height)
        max_left_height = 0
        max_right_height = 0
        for i in range(len(height)):
            max_left_height = max(max_left_height, height[i])
            left_max[i] = max_left_height
        for i in range(len(height)-1,-1,-1):
            max_right_height = max(max_right_height, height[i])
            right_max[i] = max_right_height
        for i in range(len(height)):
            ans += min(left_max[i], right_max[i]) - height[i]
        return ans


    def trapbruteforce(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        ans = 0
        for i in range(len(height)):
            max_left, max_right = 0 , 0
            for j in range(i + 1):
                max_left = max(max_left, height[j])
            for j in range(i, len(height)):
                max_right = max(max_right, height[j])
            ans += min(max_left, max_right) - height[i]
        return ans
```

The main idea of this question is to find the container which can hold the water

The height of container is based on the shortest side

Therefore, we need to find both sides for a container and then find the shortest side



