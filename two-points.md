Given a string, find the length of the**longest substring**without repeating characters.

**Examples:**

Given`"abcabcbb"`, the answer is`"abc"`, which the length is 3.

Given`"bbbbb"`, the answer is`"b"`, with the length of 1.

Given`"pwwkew"`, the answer is`"wke"`, with the length of 3. Note that the answer must be a**substring**,`"pwke"`is asubsequenceand not a substring.

```py
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        chars = list(s)
        l = 0
        r = 0
        hc = {}
        maxlen = 0
        while r < len(chars):
            #print('maxlen', maxlen, 'l:', l, 'r:', r)
 
                hc[chars[r]] = r
                dist = abs(r-l+1) if l >=0 else abs(r-l)
                if dist > maxlen:
                    maxlen = dist
            else:
                l = max(hc[chars[r]] + 1, l)
                hc[chars[r]] = r
                dist = abs(r - l + 1) if l >= 0 else abs(r - l)
                if dist > maxlen:
                    maxlen = dist
            r += 1
        return maxlen
```



