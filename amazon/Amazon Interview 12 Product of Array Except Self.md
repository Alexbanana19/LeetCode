# Amazon Interview 12: Product of Array Except Self

Given an array `nums` of *n* integers where *n* > 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Example:**

```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

**Note:** Please solve it **without division** and in O(*n*).

**Follow up:**
Could you solve it with constant space complexity? (The output array **does not** count as extra space for the purpose of space complexity analysis.)



## Solution : Two Sweeps, Time O(N), Space O(1)

The idea is simple and brilliant. Basically we just need to get the production before the element in the first sweep, then get the production after the element at the second iter. The result will be the production of the subproductions from those two rounds.



### Code

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        output = []
        p = 1
        
        for i in xrange(len(nums)):
            output.append(p)
            p *= nums[i]
        
        p = 1
        
        for i in xrange(len(nums)-1, -1, -1):
            output[i] *= p
            p *= nums[i]
        
        return output
```



