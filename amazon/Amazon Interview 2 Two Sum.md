# Amazon Interview 2: Two Sum

## Description

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

 

## Solution 1: Sort, Time O(NlogN), Space O(N)

### Code

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        tup = [(i,num) for i, num in enumerate(nums)]
        tup = sorted(tup, key=lambda x: x[1]) # sort the indices and values
        idx, nums = list(zip(*tup))
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] > target:
                    break
                if nums[i] + nums[j] == target:
                    return [idx[i], idx[j]]
```



## Solution 2: Hash Map, Time O(N), Space O(N)

``` python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hash_map = {}
        for i, num in enumerate(nums):
            if target-num in hash_map:
                return [hash_map[target-num], i]
            hash_map[num] = i
```





