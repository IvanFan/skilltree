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

[![](https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif)](https://en.wikipedia.org/wiki/File:Sieve_of_Eratosthenes_animation.gif)

Sieve of Eratosthenes: algorithm steps for primes below 121 \(including optimization of starting from prime's square\).



