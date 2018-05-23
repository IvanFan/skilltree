```py
class Solution:
    def backTrack(self, l1, l2, extra_num, index, ln):
        new_val = l1[index] + l2[index] + extra_num
        new_extra_num = 0
        if new_val >= 10:
            new_extra_num = 1
            new_val -= 10
        ln.append(new_val)
        if index < len(l1)-1:
            self.backTrack(l1, l2, new_extra_num, index +1, ln)
        return ln

    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        
        values = self.backTrack(list(l1.val), list(l2.val), 0, 0, [])
        str_num = "".join(values)
        return ListNode(int(str_num))
        
```



