# Amazon Interview 13: First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```



**Note:** You may assume the string contain only lowercase letters.



## Solution : Hash Table, Time O(N), Space O(1)

I am NOT gonna explain the solution here as this problem is too classic and often appears in the interview. The main purpose of this post is to show off python's black magic-- the sugar syntax-- and how crazy we can go with such powerful weapon.



### Code

```python
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """

        return min([s.find(c) for c in string.ascii_lowercase if s.count(c)==1] or [-1])
    	# The built-in functions are actually calling C codes at its backend, which is much faster than the builing-a-hash-table-and-loop-over-twice solution. 
```



