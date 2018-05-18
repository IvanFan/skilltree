# Fraction to Recurring Decimal

Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

**Example 1:**

```
Input:
 numerator = 1, denominator = 2

Output:
 "0.5"
```

**Example 2:**

```
Input:
 numerator = 2, denominator = 1

Output:
 "2"
```

**Example 3:**

```
Input:
 numerator = 2, denominator = 3

Output: 
"0.(6)"
```

```py
class Solution:
    def fractionToDecimal(self, numerator, denominator):
        """
        :type numerator: int
        :type denominator: int
        :rtype: str
        """
        if numerator == 0:
            return str(0)
        n, remainder = divmod(abs(numerator), abs(denominator))
        sign = '-' if numerator * denominator < 0 else ''
        stack = {}
        count = 0
        res = [sign, str(n), '.']
        while remainder not in stack:
            stack[remainder] = count
            count +=1
            n, remainder = divmod(remainder*10, abs(denominator))
            res.append(str(n))
        pos = stack[remainder]
        res.insert(pos+3,'(')
        res.append(')')
        return ''.join(res).replace('(0)','').rstrip('.')
        
            
            
        
```



