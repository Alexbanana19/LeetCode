# Amazon Interview 1: Reverse Words in a String II

## Description

Given an input string , reverse the string word by word. 

**Example:**

```
Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```

**Note:** 

- A word is defined as a sequence of non-space characters.
- The input string does not contain leading or trailing spaces.
- The words are always separated by a single space.

**Follow up:** Could you do it *in-place* without allocating extra space?



## Solution 1: In-place solution, Time O(n), Space O(1)

two steps:

1. reverse each word
2. reverse the list

### Code

```python
class Solution(object):
    def reverseWords(self, str):
        """
        :type str: List[str]
        :rtype: None Do not return anything, modify str in-place instead.
        """
        def reverse(s, lo, hi):
            while lo < hi:
                tmp = str[lo]
                str[lo] = str[hi]
                str[hi] = tmp
                lo += 1
                hi -= 1
                
        lo = 0
        hi = 1
        while hi <= len(str) and lo < len(str):
            if hi == len(str) or str[hi] == " ":
                reverse(str, lo, hi-1)
                lo = hi + 1
                hi = lo + 1
            else:
                hi += 1
        reverse(str, 0, len(str)-1)
```



## Solution 2: Pythonic solution, Time O(N), Space O(N)

``` python
class Solution(object):
    def reverseWords(self, str):
        """
        :type str: List[str]
        :rtype: None Do not return anything, modify str in-place instead.
        """
        str[:] = list(' '.join(reversed(''.join(str).split(" "))))
```





