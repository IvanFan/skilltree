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

