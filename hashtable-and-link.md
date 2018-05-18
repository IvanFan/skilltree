# Hashtable and Link

# 

link architecture:

head =&gt; node =&gt; node =&gt; node =&gt; tail

hastable:

key =&gt; value

key =&gt; value

key =&gt; value

Obviously, these two data structures have sth in common.

The hashtable can become a link

e.g.

hashtable\[node1\] =&gt; node1

node1.next =&gt; node2

hashtable\[node2\] =&gt;  node2



```py
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution(object):

    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """
        dic = collections.defaultdict(lambda: RandomListNode(0))
        dic[None] = None
        n = head
        while n:
            dic[n].label = n.label
            dic[n].next = dic[n.next]
            dic[n].random = dic[n.random]
            n = n.next
        return dic[head]
        
```



