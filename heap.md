Above is a simple solution, sort the elements directly which has nlogn complex

We can also do it with heap

1. directly push everything into heap
2. while heap is not empty, get the smallest value

or we can first push the top round into the heap and if we get the smallest element, then we push the next element into the heap

Heap is used to get largest or smallest value with nLogk

---

The idea of this question is quite popular

1. Find all cirtical points \(e.g. start and end points\) and put the point and it's relevant events into the hash together

2. go through all cirtical points, for each point, find all relevant events of it

3. if it's the start of the event, push the event into heap. If it's end of the event, remove the event.

4. after the operations, check how many events are in the heap. Then based on the condition to do some operations

---

The idea of this question is to hold two heaps:

a heap contains all elements from smallest to mid

a heap contains all elements from largest to mid

Therefore, if a new element come in, we first push it into small part

Then get the largest element from small part and push it into larger part

At the same time we need to keep the length of small part &gt;= larger part and the max difference is 1

So if length of small part &lt; larger part, we need to get the smallest element from larger part and push it into the smallest part

---

