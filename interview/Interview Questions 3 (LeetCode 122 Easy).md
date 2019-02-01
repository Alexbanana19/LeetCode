# Interview Questions 3 (LeetCode 122 Easy)

## Description

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

![126_example](C:\Users\Alex\Desktop\leetcode\interview\doc\126_example.png)

## Solution 1 Dynamic Programming (Time O(n^2), Space O(n))

The most simple idea is to use dynamic programming:

- The maximum profits made before and on day B:
  -  Profits(B) = max_i(max(Price(day B) - Price(day A_i), 0) + Profits(A_i-1))
- Keep a hash Table called **Profits** to avoid repeated computation

### Code

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if len(prices) <= 1:
            return 0
        
        if 'profits' not in self.__dict__.keys(): # initialize hash table
            self.profits = [-1 for p in prices]
        
        if self.profits[len(prices)-1] != -1:
            return self.profits[len(prices)-1]
        
        max_profits = 0 # find the max profits
        for i in range(len(prices)):
            profits = max(prices[-1]-prices[i], 0) + self.maxProfit(prices[:i])
            if profits > max_profits:
                max_profits = profits
        
        # store the value in the hash table        
        self.profits[len(prices)-1] = max_profits 
        return max_profits
```



## Solution 2 Find Increasing Subsequences (Time O(n), Space O(1))

However, we can find a much simpler and faster solution using a special property of the problem.

The problem is actually asking us to find all the increasing subsequences.

Suppose 

- if the sequence is "a <= b <= c <= d", the profit is "d - a = (b - a) + (c - b) + (d - c)" without a doubt. 
- elif the sequence is "a <= b >= b' <= c <= d", the profit is not difficult to be figured out as "(b - a) + (d - b')". So you just target at monotone sequences.

### Code

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        return sum([max(prices[i+1] - prices[i],0) 
                        for i in range(len(prices)-1)])
```

