# Binary Tree

Deep-first search

**Preorder:**

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Sorted_binary_tree_preorder.svg/220px-Sorted_binary_tree_preorder.svg.png)](https://en.wikipedia.org/wiki/File:Sorted_binary_tree_preorder.svg)

Pre-order: F, B, A, D, C, E, G, I, H.

1. Check if the current node is empty or null.
2. Display the data part of the root \(or current node\).
3. Traverse the left subtree by recursively calling the pre-order function.
4. Traverse the right subtree by recursively calling the pre-order function.





Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```



