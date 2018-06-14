![](/assets/Screen Shot 2018-06-14 at 2.14.01 pm.png)



```py
import math

class Solution:
    def evalRPN(self, tokens):
        """
        :type tokens: List[str]
        :rtype: int
        """
        stack = []
        tools = {
            '+':'+',
            '-':'-',
            '*': '*',
            '/':'/'
        }
        for i in tokens:
            if i not in tools.keys():
                stack.append(int(i))
            else:
                first_number = stack.pop()
                second_number = stack.pop()
                if i == '+':
                    stack.append(first_number + second_number)
                elif i == '-':
                    stack.append(second_number - first_number)
                elif i == '*':
                    stack.append(int(first_number * second_number))
                elif i == '/':
                    if first_number == 0:
                         stack.append(0)
                    else:
                        result = int(second_number / first_number)
                        # if result > 0:
                        #     result = math.floor(result)
                        # else:
                        #     result = math.ceil(result)
                        stack.append(result)
        return stack.pop()
                
```



