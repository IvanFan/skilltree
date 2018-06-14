![](/assets/Screen Shot 2018-06-14 at 2.58.41 pm.png)

```py
import re

class Solution:
    def cal(self, op, second_num, first_num):
        if op == '+':
            return first_num + second_num
        elif op == '-':
            return first_num - second_num
        elif op == '*':
            return first_num * second_num
        elif op == '/':
            return int(first_num / second_num)

    def is_digit(self, n):
        try:
            int(n)
            return True
        except ValueError:
            return False

    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        ops_stack = []
        num_stack = []
        level = { '+': 1, '-': 1, '*': 2, '/':2, '(': 0, ')': 0 }
        for i in re.compile('\d+|[(+-/\*)]').findall(s):
            if i in level.keys():
                if len(ops_stack) == 0 or i == '(' or level[i] > level[ops_stack[-1]]:
                    ops_stack.append(i)
                elif i == ')':
                    while ops_stack[-1] != '(':
                        num_stack.append(self.cal(ops_stack.pop(), num_stack.pop(), num_stack.pop()))
                    ops_stack.pop()
                elif level[i] <= level[ops_stack[-1]]:
                    num_stack.append(self.cal(ops_stack.pop(), num_stack.pop(), num_stack.pop()))
                    ops_stack.append(i)
            else:
                num_stack.append(int(i))

        while len(ops_stack) != 0:
            num_stack.append(self.cal(ops_stack.pop(), num_stack.pop(), num_stack.pop()))
        return num_stack[0]
```

This question is similar to the previous question, But more complex

First the ops has different priority. Second we need to consider the bracket



