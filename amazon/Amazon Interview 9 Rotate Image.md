# Amazon Interview 9: Rotate Image

## Description

You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**Example 2:**

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```



## Solution 1: Copy, Time O(N), Space O(N)

Copy the original matrix and once we start to rotate the matrix, we copy elements from the copy to the target place in the original matrix. The mapping is: [i, j] -> [j, N-1].

### Code

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        N = len(matrix)
        matrix_cop = [[matrix[i][j] for j in range(N)] for i in range(N)]
        
        for i in range(N):
            for j in range(N):
                src = (i, j)
                tar = (j, N-1-i)
                
                matrix[tar[0]][tar[1]] = matrix_cop[src[0]][src[1]]
```



## Solution 1: Copy, Time O(N), Space O(1)

The idea is simple and elegant, and a quite common method for image rotation, which involves two steps that are interchangeable.

(1) rotate the rows/columns;

(2) swap the elements using diagonal as the axis.



### Code

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        
        n = len(matrix)
        # flip horizontally
        for i in range(n):
            for j in range(n/2):
                matrix[i][n - 1- j], matrix[i][j] = matrix[i][j], matrix[i][n - 1 - j]
        # swap symmetrically
        for i in range(n):
            for j in range(i + 1, n):
                matrix[j][i], matrix[i][j] = matrix[i][j], matrix[j][i]
```

