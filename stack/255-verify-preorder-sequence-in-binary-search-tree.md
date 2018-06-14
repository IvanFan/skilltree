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

The main idea is to use the url

left node &lt; root &lt; right

Therefore,  we just need to find the the right side of the tree

And make sure that all left nodes&lt; root and right nodes &gt; root

if failed, then it's incorrect

So the idea is to find the node val &gt; than the last node val 

It means we find the right tree, then we go back to find the root

all elements after this right tree should larger than root 

