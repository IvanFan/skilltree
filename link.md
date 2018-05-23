```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def backTrack(self, l1, l2, extra_num):
        new_val = l1.val + l2.val
        new_node = ListNode() 
        return new_node 
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        return self.backTrack(l1, l2, 0)

```



