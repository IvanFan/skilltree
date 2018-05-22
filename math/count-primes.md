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





To find all the prime numbers less than or equal to 30, proceed as follows.

First, generate a list of integers from 2 to 30:

```
 2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30

```

The first number in the list is 2; cross out every 2nd number in the list after 2 by counting up from 2 in increments of 2 \(these will be all the multiples of 2 in the list\):

```

```

The next number in the list after 2 is 3; cross out every 3rd number in the list after 3 by counting up from 3 in increments of 3 \(these will be all the multiples of 3 in the list\):

```

```

The next number not yet crossed out in the list after 3 is 5; cross out every 5th number in the list after 5 by counting up from 5 in increments of 5 \(i.e. all the multiples of 5\):

```

```

The next number not yet crossed out in the list after 5 is 7; the next step would be to cross out every 7th number in the list after 7, but they are all already crossed out at this point, as these numbers \(14, 21, 28\) are also multiples of smaller primes because 7 × 7 is greater than 30. The numbers not crossed out at this point in the list are all the prime numbers below 30:

```
 2  3     5     7           11    13          17    19          23                29
```



