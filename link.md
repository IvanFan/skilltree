```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def backTrack(self, l1, l2, en):
        if not l1 and en != 0:
           return ListNode(en)
        elif not l1:
           return None
        value = l1.val + l2.val + en
        ten = value // 10 
        new_val = value % 10
        new_node = ListNode(new_val)
        new_node.next = self.backTrack(l1.next, l2.next, ten)
        return new_node

    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        return self.backTrack(l1, l2, 0)
        
```



