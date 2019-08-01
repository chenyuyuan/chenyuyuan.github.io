---
title: spto-10-fibonacci
date: 2019-07-30 23:21:58
tags: spto
---

##### description:

求Fibonacci序列

##### code in java:

```java
public static int recursiveFibonacci(int n){
    if(n <= 0) {
        return 0;
    }
    if(n == 1) {
        return 1;
    }
    return recursiveFibonacci(n - 1) + recursiveFibonacci(n - 2);
}
public static int iteratorFibonacci(int n){
    int a = 0;
    int b = 1;
    if(n <= 0) {
        return 0;
    }
    if(n == 1) {
        return 1;
    }
    for(int i = 0;i < n - 1;++i) {
        b = a + b;
        a = b - a;
    }
    return b;
}
```

