In python, we use list to represent stack. So stack is just a list

Then we can use all operation of list for stack

e.g. list.pop\(\) min\(list\)

---

Since we need to use both side and we also want to do it once for all

Therefore we need to save the previous values for a certain period until it has been used

Stack is the best option for this situation

From left to right, if the height is decreasing, add to the stack

In summary,  we need to find the previous left sides. So we save those data into the stack and setup a condition to calculate the left and right sides of the container

---

This is a classic problem for stack. We also have more complex problem similar to this

But the main idea is the same, save previous values in to the stack and setup a condition to handle the previous values

---

This question has a lot of solution. Usually when it related to getting max or min value, we can use heap.

Heap is good for unsorted data. For this question we know it's a binary search tree. It means that we can go through the tree and get a sorted list.

Then we can push all element into the list and pop it out

---

The idea is the find the smallest letter and put it at the beginning.

Then the problem is,  how can we make sure that all letter we have met previously will occur again?

so we now convert the problem to:

we need to check if all previous values can occur again in the future

The idea is to use a hash table to calculate the number of all letters.

Since we need to use the previous values under certain condition, we can use a stack to hold them

---

The idea of this question is clean.

Saved values in to the stack and do the calculation under certain condition.

It doesn't have \* and /  so it's actually eaay.

The only issue is : one number can hold multiple chars

---

This question is similar to the previous question, But more complex

First the ops has different priority. Second we need to consider the bracket

e.g. we need to calculate the content within the bracket

we also need to consider the ops

So it's a more complex situation for saving previous values and handle it under conditions

1. we need to handle the bracket
2. we need to calculate the \* and / before + and -

Therefore, we can save the ops and numbers in two stacks

If there is no + and  - after \* and /, we can do the calculate just like before

So we have two conditions:

1. if we meet \) 
2. if we meet -and + after \* and /

---

The target of this problem is to find the left and right side of the current height.

The first height &lt; the current height is the side of the current height

---

