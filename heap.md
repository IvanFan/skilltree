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

