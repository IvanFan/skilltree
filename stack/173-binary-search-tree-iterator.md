![](/assets/Screen Shot 2018-06-14 at 2.18.16 pm.png)

```py
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    stack = []
    pool_size = 0
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.stack = []
        curr = root
        self.backTrace(curr)
        self.pool_size = len(self.stack)

    def backTrace(self, node):
        if not node:
            return 
        if node:
            self.backTrace(node.right)
            self.stack.append(node.val)
            self.backTrace(node.left)

        return


    def hasNext(self):
        """
        :rtype: bool
        """
        if self.pool_size != 0:
            return True
        else:
            return False


    def next(self):
        """
        :rtype: int
        """
        if self.pool_size != 0:
            self.pool_size -=1 
            return self.stack.pop()



# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
```

This question has a lot of solution. Usually when it related to getting max or min value, we can use heap.

Heap is good for unsorted data. For this question we know it's a binary search tree. It means that we can go through the tree and get a sorted list.

Then we can push all element into the list and pop it out

