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

So our target is to find both side of the container:

**BruteForce:**

for each point, find the max left and max right and then find the min side

Obviously it's n^2

**Dynamic programming:**

From the previous solution, we can optimize the performance e.g. use space to get better time complexity

calculate max left for each point

calculate max right for each point

the calculate the height for each point

It's N

**Stack:**

Since we need to use both side and we also want to do it once for all

Therefore we need to save the previous values for a certain period until it has been used

Stack is the best option for this situation

From left to right, if the height is decreasing, add to the stack

```
while the height is higher than the last element of the stack, then we need to handle the container size calculation
    pop the element from the stack, this is the current bottom of the container. Then the last element of the stack is the left side of the current container.
    the size of hold water will be the container width X min height of left side and right side
    this will calculate all containers from right to left
add the current height to the stack 
```

In summary,  we need to find the previous left sides. So we save those data into the stack and setup a condition to calculate the left and right sides of the container

