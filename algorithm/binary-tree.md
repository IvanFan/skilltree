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

* merge sort

* quick sort

* most of the binary Tree problem





