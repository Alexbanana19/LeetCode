# Amazon Interview 5: Longest Substring Without Repeating Characters

## Description

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```



## Solution 1 (self): Two Pointers, Time O(N), Space O(N)

- The basic idea is to use a hash map to record every character while iterating through the string. 

- We use the first pointer to iterate through the string, and if the character it points to is already in the hash_map, we move the second pointer to the right.

- Remove the corresponding character from the hash map until the char that pointed by the first pointer is no longer in the hash_map.

### Code

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        hash_map = {}
        start = 0 # second pointer
        max_len = 0
        for i in range(len(s)): # i is the first pointer
            char = s[i]
            if char in hash_map:
                while hash_map[char] > 0 and start <= i:
                    hash_map[s[start]] -= 1
                    start += 1
            hash_map[char] = 1
            
            if i - start + 1 > max_len:
                max_len = i - start + 1
        return max_len
```



## Solution 2 : DP-like Solution, Time O(N), Space O(N)

The solution is similar to the first one, but instead of moving the second pointer until the char pointed by the first pointer is no longer in the hash map, we directly move the second pointer to the right-hand side of the latest repeated char.

### Code

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash_map = {}
        start = 0 # second pointer
        max_len = 0
        for i in range(len(s)): # i is the first pointer
            if s[i] in hash_map:
                start = max(hash_map[s[i]]+1, start) # direct jump
            hash_map[s[i]] = i # record index
            if max_len < i-start+1: max_len = i-start+1
        return max_len
            
            
                    
                
```

