# Amazon Interview 8: 3Sum Closest

## Description

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```



## Solution: Sorting (Two Pointers), Time O(N^2), Space O(N)

Basically it's the same as the 3 sums solution: we use three pointers to keep track of the current ele, the leftest ele and the rightest ele. And then we update the closest sums at each iteration

### Code

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        res = nums[0] + nums[1] + nums[2]
        for i in range(len(nums)-2):
            if i > 0 and nums[i-1] == nums[i]:
                continue
            l,r = i+1, len(nums)-1
            
            while l < r:
                sums = nums[i] + nums[l] + nums[r]
                if sums == target:
                    return sums
                
                if abs(sums - target) < abs(res - target):
                    res = sums
                
                if sums < target:
                    l += 1
                
                elif sums > target:
                    r -= 1
        return res
```

