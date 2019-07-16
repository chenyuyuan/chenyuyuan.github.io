---
title: leetcode-263-ugly-number
date: 2019-07-16 15:27:58
tags: leetcode

---

### leetcode 263. ugly number

description:

Write a program to check whether a given number is an ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.

**Example 1:**

```
Input: 6
Output: true
Explanation: 6 = 2 × 3
```

<!-- more -->

**Example 2:**

```
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
```

**Example 3:**

```
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```

##### solve:

num分别整除2，3，5，直到不能整除

finally，if num == 1，return true，else if num != 1，return false;



##### code: (java solution)

```java
class Solution {
    public boolean isUgly(int num) {
        while (num % 2 == 0 && num != 0)
            num /= 2;
        while (num % 3 == 0 && num != 0)
            num /= 3;
        while (num % 5 == 0 && num != 0)
            num /= 5;
        return num == 1;
    }
}
```







