# Binary Tree

Binary Tree DFS Traversal

* preorder/inorder/postorder
* Divide & conquer
* DFS Template

Binary Tree BFS Traversal

* BFS template

Binary Search Tree

* validate, insert, delete

## Preorder Traversal

**No recursion**

**Traversal**

```
root
traversal(root.left, result)
traversal(root.right, result)
```

**Divide & Conquer**

tips: traversal need to pass result during the recursion but divide and conquer use return value

```
def preorder_traversal(root):
     result = []
     # none or leaf
     if not root:
        return None
     # Divide
     left = preorder_traversal(root.left)
     right = preorder_traversal(root.right)
     # conquer
     result += root.val
     result += left
     result += right
     return result
```

## Divide Conquer Algorithm

* merge sort O\(nlogn\)

* quick sort O\(nlogn\)

* most of the binary Tree problem

**Merge Sort**

![](/assets/Screen Shot 2018-06-30 at 3.12.51 pm.png)

无论如何我在中间切一刀，然后吧左边进行merge sort，右边进行merge sort

然后吧左右两边的sorted array 合并到一个新的数组

然后吧新数组copy 和覆盖回原来的数组

merge sort 会耗费额外空间 先局部有序然后整体有序

Quick  sort has no need for extra space

**Quick sort **

1. randomly pickup one number in the array and get the value X
2. then partition the array in place to two arrays 
3. so we will try to sort the left part to be &lt;= x and right part to be &gt; x
4. then we sort left and right

so we first make the entire array ordered and then make partition sorted

Quick nlogn O\(1\) 如果key一样的时候排序完成顺序会变掉

Merge nlogn O\(n\) if key is the same, the original order will not be changed

Analysis the time complexity

Merge Sort:![](/assets/Screen Shot 2018-07-01 at asdfasdf pm.png)Quick sort

the randomly selected x may not be the mid of the array

the worst case is O\(n^2\)

the normal case is: O\(nlogn\)

## Maximum depth of binary tree

```
if not root:
  return 0
left =  findMax(root.left)
right = findMax(root.right)
return max(left, right) +1
```

## Balanced Binary Tree

```
python 可以return tuple or return -1
```

binary search for this one: O\(N\) because each operation is O\(1\) and we only go through n node

## Binary Tree Maximum Path Sum

Given a**non-empty**binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain**at least one node**and does not need to go through the root.

**Example 1:**

```
Input:
 [1,2,3]


1
/ \
2
3
Output:
 6
```

**Example 2:**

```
Input:
 [-10,9,20,null,null,15,7]

   -10
   / \
  9  
20
/  \
15   7
Output:
 42
```

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import sys
class Solution:

    def findMaxPath(self, root):
        if root and not root.left and not root.right:
            self.max_val = max(self.max_val, root.val)
            return root.val
        elif root and root.left and root.right:
            left = self.findMaxPath(root.left)
            right = self.findMaxPath(root.right)
            left_tem = left + root.val
            right_tem = right + root.val
            merged_tem = right + root.val + left
            self.max_val = max(self.max_val, max([left_tem, right_tem, merged_tem, root.val]))
            return max([left_tem, right_tem, root.val])
        elif root and root.left and not root.right:
            left = self.findMaxPath(root.left)
            left_tem = left + root.val
            self.max_val = max(self.max_val, max([left_tem, root.val]))
            return max([left_tem, root.val])
        elif root and not root.left and root.right:
            right = self.findMaxPath(root.right)
            right_tem = right + root.val
            self.max_val = max(self.max_val, max([right_tem, root.val]))
            return max([right_tem, root.val])

    def maxPathSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.max_val = root.val
        self.findMaxPath(root)
        return self.max_val
```

tips:

max left max right max left-root-right



