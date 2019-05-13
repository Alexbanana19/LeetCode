# Amazon Interview 7: 3Sum

## Description

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



## Solution: Sorting, Time O(N^2), Space O(1)

The problem is similar to two sum, but instead of being given a fixed target, we use the current element as the target while iterating through the array. Of course, we can use the traditional two sum solution. However, we need to eliminate duplicate at the end. The following code provides a brilliant way to avoid duplicate.

### Code

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort() # sort the array
        res = []
        for i in range(len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]: # avoid duplicates in the answer
                continue
                
            l, r = i + 1, len(nums)-1
            
            while l < r:
                sums = nums[i] + nums[l] + nums[r]
                if sums < 0: # key
                    l += 1
                elif sums > 0:
                    r -= 1
                else:
                    res.append([nums[i], nums[l], nums[r]])
                    while l < r and nums[l] == nums[l+1]: # avoid duplicates
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1
                    r -= 1
        return res
```

