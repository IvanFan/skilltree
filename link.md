```py
class Solution:
    def backTrack(self, l1, l2, extra_num):
        new_val = l1.val + l2.val + extra_num
        new_extra_num = 0
        if new_val >= 10:
            new_extra_num = 1
            new_val -= 10
        new_node = ListNode()
        new_node.val = new_val
        if l1.next:
            new_node.next = self.backTrack(l1.next, l2.next, new_extra_num)
        return new_node

    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        return self.backTrack(l1[0], l2[0], 0)
```



