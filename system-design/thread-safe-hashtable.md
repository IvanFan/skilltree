Tips:

hash conflicts

thread safe

Hash conflict:

![](/assets/Screen Shot 2018-06-22 at 3.52.22 pm.png)Java JDK7 and 8 are using chainning solution

Thread safe:

1. Global lock
2. Lock stripping: Divide all buckets into 16 Segments. Each Segment has one lock
3. Optimise with CAS opertaion \(Atomic  operation\) read-modify-write . If the bucket is too long, use red and black tree to read it



