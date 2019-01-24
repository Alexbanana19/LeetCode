# Interview Questions 1 (LeetCode 26 Easy)

## Description

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.



## Solution (Time O(n), Space O(1))

Because the array is already sorted, we use the double pointers method, where we use 1 pointer to keep track of the non-repeated array, while use the other to find the location of the boundaries, e.g. [...1,2...].



## Code

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return len(nums)
        
        """Orignal Solution, Swapping"""

        # for i in range(len(nums)):
        #     for j in range(i+1, len(nums)):
        #         flag = False
        #         if nums[j] <= nums[i] and j <= len(nums)-1:
        #             for k in range(j+1, len(nums)):
        #                 if nums[i] < nums[k]:
        #                     temp = nums[j]
        #                     nums[j] = nums[k]
        #                     nums[k] = temp
        #                     flag = True
        #                     break
        #             if not flag:
        #                 return i + 1
        #         break
        ind = 1
        for i in range(1,len(nums)):
            if nums[i] != nums[i-1]:
                nums[ind] = nums[i]
                ind += 1
        return ind
```

