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



