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

Above is a simple solution, sort the elements directly which has nlogn complex

We can also do it with heap

1. directly push everything into heap
2. while heap is not empty, get the smallest value

```py
from Queue import PriorityQueue

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```

or we can first push the top round into the heap and if we get the smallest element, then we push the next element into the heap

Heap is used to get largest or smallest value with nLogk

