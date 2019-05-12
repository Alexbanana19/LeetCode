# Amazon Interview 3: Maximum Size Subarray Sum Equals k 

## Description

Given an array *nums* and a target value *k*, find the maximum length of a subarray that sums to *k*. If there isn't one, return 0 instead.

**Note:**
The sum of the entire *nums* array is guaranteed to fit within the 32-bit signed integer range.

**Example 1:**

```
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4 
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

**Example 2:**

```
Input: nums = [-2, -1, 2, 1], k = 1
Output: 2 
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

**Follow Up:**
Can you do it in O(*n*) time?



## Solution : Range Sum + Hash Map, Time O(N), Space O(N)

The idea is simple: to obtain the sum over an arbitrary subarray, we just need to 

1. calculate the accumulative sum for every num
2. use hash map to check whether the difference between every two accumulative sum is equal to the target (similar to 'two sum')
3. find the subarray with the maximum length

\* Strangely, my solution only beats like 5% of solutions... wondering what the faster methods look like.

### Code

```python
class Solution(object):
    def maxSubArrayLen(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        sums, res = 0,0
        hash_map = {0:-1}
        for i, num in enumerate(nums):
            sums += num
            if sums not in hash_map:
                hash_map[sums] = i
            if sums - k in hash_map:
                res = max(res, i-hash_map[sums-k])
        return res
```

