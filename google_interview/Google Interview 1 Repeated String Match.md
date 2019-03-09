# Google Interview 1: Repeated String Match

## Description

Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").

**Note:**
The length of `A` and `B` will be between 1 and 10000.



## Solution 1:  First Thought, Non-elegant

Repeat A until its len is geq than B, and then find the match char by char. If the string is out of border, redirect the outliers to the front and increase the count.



### Code

```python
class Solution(object):
    def repeatedStringMatch(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: int
        """
        count = 0
        A_rep = ""
        while len(A_rep) < len(B): # non pythonic
            A_rep += A
            count += 1
        
        for i in range(len(A_rep)): # find match char by char, slow
            start = i
            end = i + len(B)

            if end <= len(A_rep):
                if A_rep[start:end] == B:
                    return count
            
            else: 
                end %= len(A_rep)
                if A_rep[start:len(A_rep)]+A_rep[:end] == B:
                    return count + 1
        return -1
```



## Solution 2 Elegant Solution

So basically if B is the substring of rep(A, k), then we should return k = x or x + 1, where x = ceil(len(A)//len(B)). We only need to check whether B is in rep(A, x) or rep(A, x+1).

### Code

```python
class Solution(object):
    def repeatedStringMatch(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: int
        """
        times = -(-len(B)//len(A)) # an implementaion of ceil(len(A)//len(B)) without the library
        for i in range(2):
            if B in A*(i+times):
                return times + i
        return -1
```

