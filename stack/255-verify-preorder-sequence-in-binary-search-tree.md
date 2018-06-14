![](/assets/Screen Shot 2018-06-14 at 4.49.32 pm.png)



```py
class Solution:
    def verifyPreorder(self, preorder):
        """
        :type preorder: List[int]
        :rtype: bool
        """
        if len(preorder) == 0:
            return True
        stack = []
        lower_board = None
        for i in preorder:
            if lower_board is not None and i < lower_board:
                return False
            if len(stack) == 0 or stack[-1] > i:
                stack.append(i)
            elif len(stack) != 0 and stack[-1] < i:
                curr = stack.pop()
                while len(stack) != 0 and stack[-1] < i:
                    curr = stack.pop()
                lower_board = curr
                stack.append(i)
        return True
                    
        
        
```



