```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def backTrack(self, l1, l2, extra_num, index):
        new_val = l1[index] + l2[index] + extra_num
        new_extra_num = 0
        if new_val >= 10:
           new_extra_num = 1
           new_val -= 10
        new_node = ListNode() 
        new_node.val = new_val
        if index < len(l1)-1:
           new_node.next = self.backTrack(l1, l2, new_extra_num, index+1)
        return new_node 

    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        return self.backTrack(l1, l2, 0, 0)
```



