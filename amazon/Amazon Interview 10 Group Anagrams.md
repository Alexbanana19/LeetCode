# Amazon Interview 10: Group Anagrams

Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

- All inputs will be in lowercase.
- The order of your output does not matter.



## Solution 1: Hash + Sort, Time O(N*MlogM), Space O(?)

Unarguably, the best solution to this problem is hashing which maps the anagrams to the same key. So the problem converts to how we choose our hashing function. For this solution, we sort the incoming string so the anagrams will have the same key. N is the number of strings and M is the average length of the strings.

### Code

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        d = {}
        for w in strs:
            key = tuple(sorted(w))
            d[key] = d.get(key, []) + [w]
        return d.values()
```



## Solution 2: Hash + Prime Numbers, Time O(N*M), Space O(1)

The previous solution solves the problem of collision while hashing, however, it takes O(MlogM) to sort the string. Is there a O(M) method that we can apply? Fortunately, the following solution provides a brilliant way to create unique hash keys for the anagrams, which is using prime numbers. We first assign each character with a prime number, and then use the production of those numbers as the key. However, this solution has several issues: 1. overflow when encountering strings like "yyyyyyyy" 2. can only deal with situations when all the characters is known.

### Code

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        primes = [2, 3, 5, 7, 11 ,13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 107]
        d = {}
        for w in strs:
            if w is "":
                d[0] = d.get(0, []) + [w]
            else:
                key = 1
                for c in w:
                    key *= primes[ord(c)-ord('a')]
            d[key] = d.get(key, []) + [w]
        return d.values()
```



