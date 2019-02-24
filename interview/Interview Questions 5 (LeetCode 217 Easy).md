# Interview Questions 5 (LeetCode 217 Easy)

## Description



**The question seems to be a piece of cake, but let's face it, you always forget the elegant solution :)**



Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

![126_example](C:/Users/Alex/Desktop/leetcode/interview/doc/217_example.png)





## Solution 1 Sorting (Time O(nlgn), Space O(n))

### Code

```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        nums = sorted(nums);
        for i in range(1,len(nums)): 
            if nums[i-1] == nums[i]:
                return True
        return False
```



## Solution 2 Hash Linked List (Time O(n), Space O(n))

### Code

```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        length = min(7919,len(nums)) # prime number
        dic = [[] for i in range(length)]
        for num in nums:
            idx = num%length
            if len(dic[idx]) == 0:
                dic[idx].append(num)
            else:
                for d in dic[idx]:
                    if d == num:
                        return True
                dic[idx].append(num)
        return False
```



## Solution 3 Pythonic Solution: Set (Time O(n), Space O(n))

### Code

```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        # burned
        return len(nums) != len(set(nums))
```

