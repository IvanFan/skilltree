![](/assets/Screen Shot 2018-06-14 at 2.33.13 pm.png)

```py
class Solution:
    def is_digit(self, n):
        try:
            int(n)
            return True
        except ValueError:
            return  False
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        stack = []
        s = s.replace(' ', '')
        ops = { '+': '+', '-': '-'}
        buckets = { '(':')' }
        count = 0
        char = s[count]
        while count < len(s):
            # print(stack)
            # print(char)
            if len(stack) == 0 or ((stack[-1] not in ops.keys() and char not in buckets.values()) or char in buckets.keys()):
                stack.append(char)
                count += 1
                char = s[count] if count < len(s) else None
            elif len(stack) != 0 and stack[-1] in ops.keys() and self.is_digit(char):
                current_number = char
                while count+1 < len(s) and self.is_digit(s[count+1]):
                    count += 1
                    char = s[count]
                    current_number += char
                curr_op = stack.pop()
                last_number = stack.pop()
                while len(stack) != 0 and self.is_digit(stack[-1]):
                    last_number = stack.pop() + last_number
                result = 0
                if curr_op == '+':
                    result = int(last_number) + int(current_number)
                elif curr_op == '-':
                    result = int(last_number) - int(current_number)
                char = str(result)
            elif len(stack) != 0 and char in buckets.values():
                last_number = stack.pop()
                while len(stack) != 0 and self.is_digit(stack[-1]):
                    last_number = stack.pop() + last_number
                front_bucket = stack.pop()
                char = last_number

        last_number = stack.pop()
        while len(stack) != 0 and self.is_digit(stack[-1]):
            last_number = stack.pop() + last_number
        return int(last_number)
```



The idea of this question is clean. 

Saved values in to the stack and do the calculation under certain condition. 

It doesn't have \* and /  so it's actually eaay.

The only issue is : one number can hold multiple chars

