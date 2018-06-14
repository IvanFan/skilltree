![](/assets/Screen Shot 2018-06-14 at 10.43.10 pm.png)





```py
class Solution:
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
    
        nodes = []
        for i in range(len(lists)):
            curr = lists[i]
            while curr:
                nodes.append(curr.val)
                curr = curr.next
        nodes.sort()
        head = None
        for i in range(len(nodes)):
            if head is None:
                head = ListNode(nodes[i])
                head.next = None
                tem = head
            else:
                new_node = ListNode(nodes[i])
                tem.next = new_node
                tem = new_node
        return head
```



