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

#### Long division: {#approach-1-long-division-accepted}

For long division, if it's infinite number, the number will start repeating at some point.

![](/assets/Screen Shot 2018-05-18 at 5.15.17 pm.png)

#### Approach \#1 \(Long Division\) \[Accepted\] {#approach-1-long-division-accepted}

**Intuition**

The key insight here is to notice that once the remainder starts repeating, so does the divided result.

**Algorithm**

You will need a hash table that maps from the remainder to its position of the fractional part. Once you found a repeating remainder, you may enclose the reoccurring fractional part with parentheses by consulting the position from the table.

The remainder could be zero while doing the division. That means there is no repeating fractional part and you should stop right away.

Just like the question[Divide Two Integers](https://leetcode.com/problems/divide-two-integers/), be wary of edge cases such as negative fractions and nasty extreme case such as\frac{-2147483648}{-1}​−1​​−2147483648​​.

