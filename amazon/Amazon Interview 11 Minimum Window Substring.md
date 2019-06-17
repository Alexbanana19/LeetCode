# Amazon Interview 11: Minimum Window Substring

- Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

  **Example:**

  ```
  Input: S = "ADOBECODEBANC", T = "ABC"
  Output: "BANC"
  ```

  **Note:**

  - If there is no such window in S that covers all characters in T, return the empty string `""`.
  - If there is such window, you are guaranteed that there will always be only one unique minimum window in S.



## Solution : Two Pointers + Hash, Time O(N), Space O(N)

There's really nothing to say about this one as the methods are limited to two pointers + hashing. So the main idea is stated as follows:

1. initialize a hash map with elements from T and how many times they appear. We use `missing` to indicate how many more target characters are needed in the sliding window.
2. we use two pointers to denote the start and end of the window. We moves the end pointer at each iteration and when no character is missing from the window, we move the the start pointer towards the end to remove the redundant characters in the window.
3. Each character in `s` is maximally accessed twice, so the time complexity is O(N). 

### Code

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        need, missing = collections.Counter(t), len(t)
        i = I = J = 0
        for j, c in enumerate(s, 1):
            missing -= need[c] > 0 # Counter will return 0 if c not in need
            need[c] -= 1
            if not missing:
                while i < j and need[s[i]] < 0: # The negative one represents the redudant character, once we hit the positive, we find the current minimal window
                    need[s[i]] += 1
                    i += 1
                if not J or j - i <= J - I:
                    I, J = i, j
        return s[I:J]
```



