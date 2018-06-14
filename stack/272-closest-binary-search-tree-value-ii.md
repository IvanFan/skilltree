![](/assets/Screen Shot 2018-06-14 at 5.58.07 pm.png)



```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderSuccessor(self, root, target, successor):
        if root is None:
            return
        self.inorderSuccessor(root.right, target, successor)
        if root.val <= target:
            return
        successor.append(root.val)
        self.inorderSuccessor(root.left, target, successor)
        return 
        
    def inorderPredecessor(self, root, target, predecessor):
        if root is None:
            return
        self.inorderPredecessor(root.left, target, predecessor)
        if root.val > target:
            return
        predecessor.append(root.val)
        self.inorderPredecessor(root.right, target, predecessor)
        return 
    def closestKValues(self, root, target, k):
        """
        :type root: TreeNode
        :type target: float
        :type k: int
        :rtype: List[int]
        """
        predecessor = []
        successor = []
        res = []
        self.inorderPredecessor(root, target, predecessor)
        self.inorderSuccessor(root, target, successor)
        while k != 0:
            if len(predecessor) == 0 and len(successor) != 0:
                res.append(successor.pop())
            elif len(successor) == 0 and len(predecessor) != 0:
                res.append(predecessor.pop())
            elif len(predecessor) != 0 and len(successor) != 0:
                if abs(predecessor[-1] - target) < abs(successor[-1] - target):
                    res.append(predecessor.pop())
                else:
                    res.append(successor.pop())
            k -= 1
        return res
```



