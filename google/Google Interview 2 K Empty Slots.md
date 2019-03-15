# Google Interview 2: K Empty Slots (Hard)

## Description

There is a garden with `N` slots. In each slot, there is a flower. The `N` flowers will bloom one by one in `N` days. In each day, there will be `exactly` one flower blooming and it will be in the status of blooming since then.

Given an array `flowers` consists of number from `1` to `N`. Each number in the array represents the place where the flower will open in that day.

For example, `flowers[i] = x` means that the unique flower that blooms at day `i` will be at position `x`, where `i` and `x`will be in the range from `1` to `N`.

Also given an integer `k`, you need to output in which day there exists two flowers in the status of blooming, and also the number of flowers between them is `k` and these flowers are not blooming.

If there isn't such day, output -1.

**Example 1:**

```
Input: 
flowers: [1,3,2]
k: 1
Output: 2
Explanation: In the second day, the first and the third flower have become blooming.
```



## How to think about the problem

First Remark: This is a really long summary as it basically covers the most solutions to this problem. 

This type of problems typically exhibit "symmetric" features in the input data set. For this problem, we have equivalent perspectives of the input data as either `flowers` or `days`. Similar situations can be found for [lc354](https://leetcode.com/problems/russian-doll-envelopes/description/) and [lc630](https://leetcode.com/problems/course-schedule-iii/description/). However, in the presence of extra restrictions, this symmetry may be broken and solutions may favor one perspective over the other. In such cases, it would be advisable to make attempts from both perspectives and choose the one that suits you best.



## My solution: Sorted Window, Time O(nlogn)?,Space O(n)       

I hate to show this code cuz it's neither elegant nor efficient. My solution is to iterate over position (which is more clear to me than iterating over time at first) and maintain a sorted window of the `(pos, day)` tuple. Each time when the window slides over a slot, we discard the oldest element in the window and insert the incoming element using binary search.

If the distance between the position of the first two elements of the window is `k`, then return the min day of the two position. The time complexity should be `O(nlogn)`. Unfortunately, due to my insufficient implementation, I got a TLE after submitting this code :)

### Code

```python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """  	
        if len(flowers)-2 < k:
            return -1
        days = [0 for i in flowers]
        for d,p in enumerate(flowers):
            days[p-1] = d

        window = [[i, days[i]] for i in range(k+2)] # (pos, day)
        sorted_window = sorted(window, key=lambda x: x[1]) # maintain a sorted window
        indices, window = list(zip(*sorted_window))
        window = list(window)
        indices = list(indices)

        step = 0
        min_day = 999999
        while step < len(days)-k-1:
            # see if the first two element are k slots away from each other
            if abs(indices[0]-indices[1]) == k+1:
                day = max(days[indices[0]], days[indices[1]])+1
                if day < min_day:
                    min_day = day
			# delete the oldest element
            for i in range(k+2):
                if indices[i] == step:
                    del window[i]
                    del indices[i]
                    break

            step += 1

            if step >= len(days)-k-1:
                break
			# add the incoming element using binary search
            lo = 0
            hi = k
            key = step + k + 1

            while lo <= hi:
                mid = (lo+hi)//2
                if days[key] < window[mid]:
                    hi = mid - 1
                else: # no two flowers blossm at the same day
                    lo = mid + 1
            if window[mid] > days[key]:
                window.insert(mid, days[key])
                indices.insert(mid, key)
            else:
                window.insert(mid+1, days[key])
                indices.insert(mid+1, key)
        if min_day == 999999:
            return -1
        else:
            return min_day
```



## Category 1: Iterate over Time, Time O(nlogn), Space O(n)

So the basic idea is to maintain a Binary Tree and sort it in ascending order according to the positions of the visited flowers. At each day, we return the closest element to the position of that day in the set, and then check whether they are `k` slots away from each other. For convenience and clearance, I use the bisect library instead of coding from scratch.

### BST Solution

``` python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
        import bisect
        indices = [flowers[0]]
        
        for i in range(1,len(flowers)):
            idx = bisect.bisect(indices, flowers[i])
            bisect.insort(indices, flowers[i])
            if idx != 0:
                if indices[idx] - indices[idx-1] == k+1:
                    return i + 1
            if idx != len(indices)-1:
                if indices[idx+1] - indices[idx] == k+1:
                    return i + 1
        return -1
```



### BIT Solution

I m really not familiar with Binary Indexed Tree, so I will just try my best to explain the algorithm and how it fits into the solution. 

Binary Indexed tree is a tree-structured array that provides a way to calculate the prefix sums efficiently.

Just like the last solution, we need an array to keep track of the position of the visited flowers. In addition, we also need an extra array (BIT) to keep track of the total number of the blossomed flowers whose position is smaller than the current position. If the total number of the blossom flowers before `x` and  the number before `y` is the same, and the distance between `x` and `y` is` k+1`, it will mean that we find the solution. 

```python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
        def search(bit, idx):
            prefix_sum = 0
            while idx > 0:
                prefix_sum += bit[idx]
                idx -= idx & -idx # bit operation, to understand this please read about 									some BIT tutorials first
            return prefix_sum

        def insert(bit, idx):
            while idx < len(bit):
                bit[idx] += 1
                idx += idx & -idx
        
        visited = [0 for f in flowers] + [0]
        bit = [0 for f in flowers] + [0]

        for i in range(len(flowers)):
            xc = flowers[i]
            xl = xc - (k + 1) # position of the flower k slots left
            xr = xc + (k + 1) # position of the flower k slots right

            insert(bit, xc) # update BIT
            visited[xc] = 1 # update visited set

            xc_cnt = search(bit, xc)
            xl_cnt = xc_cnt - 1 # remove xl
            xr_cnt = xc_cnt + 1 # add xc 

            if (xl > 0 and visited[xl] and search(bit, xl) == xl_cnt):
                return i + 1
            if (xr <= len(flowers) and visited[xr] and search(bit, xr) == xr_cnt):
                return i + 1

        return -1
```

It is relatively easy to come up with `O(nlogn)` solutions, if you have experience with `BST` and `BIT` before, though [`O(n)`solution](https://discuss.leetcode.com/topic/104760/bucket-sort-time-o-n-space-o-n) is possible. Next we will shift our focus and take the other perspective to see if we can further improve the time performance here.



## Category 2: Iterate over Position

### Priority Queue Solution, Time O(nlogn), Space O(n)

A straightforward way to keep track of minimum blooming day of the flowers from positions `j + 2` to `i` would be using a **priority queue**. Also since the positions are changing as the candidate ranges are shifting, the priority queue should store the positions of the flowers instead of blooming days so that we can get rid of invalid positions easily. This solution is similar to my first solution, but using existing data structure is often safer and faster than coding it from scratch :)

```python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
         
        res = len(flowers) + 1
        days = [0 for f in flowers]

        for i in range(len(flowers)): # convert to position indexing
            days[flowers[i] - 1] = i + 1
        

        pq = PQueue()
        
        for i in range(len(flowers)):
            j = i - k - 1
            while not pq.empty() and pq.queue[0] <=j: # remove the old element as the 														  window slides
                pq.get()
            
            if j >=0 and (pq.empty() or days[pq.queue[0]] > max(days[i], days[j])):
                res = min(res, max(days[i], days[j])) # record the min candidate
            pq.put(i)
        return -1 if res > len(flowers) else res            
```



### Deque Solution, Time O(n), Space O(n)

It turned out that the blooming days of the flowers within the candidate range can be maintained in descending order using a double-ended queue (`deque`), **The key here is to get rid of positions with blooming days larger than that of the current position before adding it to the `deque` from the left** (this is because as long as the current position is in the `deque`, the position with minimum blooming day cannot be these removed positions). Each position will be pushed into and popped out from the `deque`once, so the overall time complexity will be `O(n)`.

```python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
         
        n = len(flowers)
        l = n # left boundary
        r = n - 1 # right boundary
        res = n + 1 # min day
        
        days = [0 for f in flowers]
        deque = [0 for f in flowers]

        for i in range(len(flowers)):
            days[flowers[i] - 1] = i + 1
        
        for i in range(len(flowers)):
            j = i - k - 1
            # get rid of the ele smaller that current pos
            while l <= r and deque[r] <= j:
                r -= 1
            
            if j >= 0 and (r < l or days[deque[r]] > max(days[i], days[j])):
                res = min(res, max(days[i], days[j]))
                
			# Get rid of larger days before adding
            while l <= r and days[i] <= days[deque[l]]: 
                l += 1
            
            # add the current position
            l -= 1
            deque[l] = i
            
        return -1 if res > len(flowers) else res
```



### No Queue Solution, Time O(n), Space O(n)

To see how we can get this `no-queue` solution (more info [here](https://discuss.leetcode.com/topic/104771/java-c-simple-o-n-solution)), let's take a look back at the two `queue`-based solutions above. The downside of those solutions is that we have to check all candidate ranges one by one, which turns out to be unnecessary. For example, if `[j, i]` is the current candidate range, `min` is the position corresponding to the minimum blooming day `d_min` in the queue such that `d_min < days[j]`, then all candidate ranges `[j', i']` with `j < j' < min` can be skipped, because the blooming day of the left boundary `days[j']` will always be greater than `d_min` and thus cannot be valid ranges.



So instead of testing all the candidate ranges one by one, we set up a target range and try to validate it by doing a linear scan, then update the target range according to the validation result. In the following Java program, the target range is denoted as `[l, r]`, with `l` and `r` as its left and right boundaries. `di`, `dl` and `dr` are the blooming days for position `i + 1`, `l + 1` and `r + 1`, respectively. To validate the target range, we need to compare `di` with `dl` and `dr`. The target range will be invalid if `di < dl` or `di < dr`, and we can skip some of the candidate ranges to reset the target range to `[i, i + k + 1]`. On the other hand, if the target range is valid, `i` will eventually be equal to `r` (or `di` will be equal to `dr`), and we need to update the final result (to find the minimum day) as well as set up a new target range. The two cases can be combined into one as shown below:

```python
class Solution(object):
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
         
        n = len(flowers)
        res = n + 1 # min day
        
        days = [0 for f in flowers]

        for i in range(len(flowers)):
            days[flowers[i] - 1] = i + 1
        
        l = 0
        r = k + 1
        i = 1
        while i < n and r < n:
            di = days[i]
            dl = days[l]
            dr = days[r]
            if di < dl or di <= dr:
                if di == dr:
                    res = min(res, max(dl, dr)) # target range is valid so update final 												  result
                l = i
                r = i + (k + 1)
            i += 1
        return -1 if res > len(flowers) else res

```

