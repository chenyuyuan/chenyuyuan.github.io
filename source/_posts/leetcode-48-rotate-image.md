---
title: leetcode-48-rotate-image
date: 2019-07-16 00:09:05
tags: leetcode
---

### leetcode 48. Rotate Image

https://leetcode.com/problems/rotate-image/

description: 

You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT**allocate another 2D matrix and do the rotation.

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

solve: 

- 矩阵顺时针旋转90°，需要将矩阵次对角线两侧的元素交换，再将矩阵上下对换，（两步顺序可以交换），

  即，将`a[i][j]` 和`a[n-j][n-i]`的元素对换，再将`a[i][j]`和`a[n-i][j]`的元素对换i, j属于[1...n]；

- 若是逆时针旋转90°，则是将矩阵主对角线两侧的元素交换，再将矩阵上下对换，（两步顺序可以交换），

  即，将`a[i][j]` 和`a[j][i]`的元素对换，再将`a[i][j]`和`a[n-i][j]`的元素对换i, j属于[1...n]；

code: (java solution)

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for(int i = 0;i < n;++i) {
            for(int j = 0;j < n -i;++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-1-j][n-1-i];
                matrix[n-1-j][n-1-i] = temp;
            }
        }
        for(int i = 0;i < n / 2;++i) {
            for(int j = 0;j < n;++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-1-i][j];
                matrix[n-1-i][j] = temp;
            }
        }
    }
}
```

