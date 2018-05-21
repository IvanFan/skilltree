# Substring

Substring duplicated issue

This issue can be solve with hashtable



Example:

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences \(substrings\) that occur more than once in a DNA molecule.

**Example:**

```
Input:
 s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"


Output:
 ["AAAAACCCCC", "CCCCCAAAAA"]

```

```py

class Solution:
    def findRepeatedDnaSequences(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        res_dict = collections.defaultdict(int)
        for i in range(len(s)-9):
            res_dict[s[i:i+10]] +=1 
        res = []
        for index in list(res_dict.keys()):
            if res_dict[index] > 1:
                res.append(index)
        return res
```



