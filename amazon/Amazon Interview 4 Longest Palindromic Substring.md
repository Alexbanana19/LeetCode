# Amazon Interview 4: Longest Palindromic Substring 

## Description

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```



## Solution 1 (self): Recursive, Time O(N^2), Space O(?)

The idea is compare the subarrays of the original and the input arrays, by shrinking one num at each step. In addition, I use cache to store the computed values to avoid redundant computation. Unfortunately, this recursive solution takes up too much memory and didn't pass the tests. 

### Code

```python
class Solution(object):
    def search(s, r):
            if len(s) <= self.max_len: # If the current string length is less than the current max_len, there's no need to compute the rest of it.
                return ""
            
            if s in self.cache: # avoid redundant computing
                return self.cache[s]
            
            if s==r:
                if len(s) > self.max_len:
                    max_len = len(s)
                return s
            
            s1 = search(s[1:], r[:-1]) # not tail-recursive, taking up too much memory
            s2 = search(s[:-1], r[1:])
            
            self.cache[s[1:]] = s1
            self.cache[s[:-1]] = s2
            
            if len(s1) > len(s2):
                return s1
            else:
                return s2
            
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        self.cache = {}
        self.max_len = 0
        r = s[::-1]

```



## Solution 2 : Seed Extension, Time O(N^2), Space O(1)



### Code

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        def extend(s, l, r):
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1
            return s[l+1:r]
        
        res = ''
        for i in range(len(s)):
            # odd
            tmp = extend(s, i, i)
            while len(tmp) > len(res):
                res = tmp
            # even
            tmp = extend(s, i, i+1)
            while len(tmp) > len(res):
                res = tmp
        return res
```



## Solution 3: Dynamic Programming, Time O(N^2), Space O(N^2)



### Code​            

```python
class Solution(object):        
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        dp = [[0] * len(s) for i in range(len(s))]
        ans = ""
        max_length = 0
        for i in range(len(s) - 1, -1, -1):
            for j in range(i, len(s)):
                if s[i] == s[j] and (j - i < 3 or dp[i+1][j-1] == 1):
                    dp[i][j] = 1
                    if ans == "" or max_length < j - i + 1:
                        ans = s[i:j+1]
                        max_length = j - i + 1
        return ans
```