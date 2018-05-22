Count Primes

Count the number of prime numbers less than a non-negative number,**n**.

**Example:**

```
Input:
 10

Output:
 4

Explanation:
 There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

```py
class Solution:
    def countPrimes(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n < 3:
            return 0
        primes = [1] * n
        primes[0] = primes[1] = 0  
        for i in range(2, int(n/2)+1):
            primes[i*i:n:i] = [0] * len(primes[i*i:n:i])
        return sum(primes)
```

# Sieve of Eratosthenes {#firstHeading}

In math, this algorithm is a simple algorithm to find all prime numbers update to any given limit





