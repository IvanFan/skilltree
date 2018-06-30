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

merge sort 会耗费额外空间











