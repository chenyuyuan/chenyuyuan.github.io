---
title: leetcode-1116-print-zero-even-odd
date: 2019-07-19 17:15:48
tags: leetcode
---

https://leetcode.com/problems/print-zero-even-odd/

##### description:

Suppose you are given the following code:

```
class ZeroEvenOdd {
  public ZeroEvenOdd(int n) { ... }      // constructor
  public void zero(printNumber) { ... }  // only output 0's
  public void even(printNumber) { ... }  // only output even numbers
  public void odd(printNumber) { ... }   // only output odd numbers
}
```

The same instance of `ZeroEvenOdd` will be passed to three different threads:

1. Thread A will call `zero()` which should only output 0's.
2. Thread B will call `even()` which should only output even numbers.
3. Thread C will call `odd()` which should only output odd numbers.

Each of the thread is given a `printNumber` method to output an integer. Modify the given program to output the series `010203040506`... where the length of the series must be 2*n*.

<!-- more -->

**Example:**

```
Input: n = 2
Output: "0102"
Explanation: There are three threads being fired asynchronously. One of them calls zero(), the other calls even(), and the last one calls odd(). "0102" is the correct output.
```

```
Input: n = 5
Output: "0102030405"
```

##### solution:

use 3 semaphore, one semaphore for print 0, two for set where print even or odd number. 

zero()函数获取szero信号量，打印0，然后根据接下来应该打印奇数或者偶数，释放s1或s2；

even()函数获取s1信号量，打印相应偶数，然后释放szero信号量，使得接下来可以接着打印0；

odd()函数获取s2信号量，打印相应偶数，然后释放szero信号量；

##### code in java:

```java
import java.util.concurrent.*;
class ZeroEvenOdd {
    private int n;
    private Semaphore szero = new Semaphore(1);
    private Semaphore s1 = new Semaphore(0);
    private Semaphore s2 = new Semaphore(0);
    public ZeroEvenOdd(int n) {
        this.n = n;
    }
    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for(int i = 0;i < n;++i) {
            szero.acquire();
            printNumber.accept(0);
            if(i % 2 == 1) {
                s1.release();
            }
            else {
                s2.release();
            }
        }
    }
    public void even(IntConsumer printNumber) throws InterruptedException {
        for(int i = 2;i <= n;) {
            s1.acquire();
            printNumber.accept(i);
            i=i+2;
            szero.release();
        }
    }
    public void odd(IntConsumer printNumber) throws InterruptedException {
        for(int i = 1;i <= n;) {
            s2.acquire();
            printNumber.accept(i);
            i=i+2;
            szero.release();
        } 
    }
}
```

note:

在acquire()的顺序有变的情况下，leetcode会报 *java You passed invalid value. Exiting.* 不知道是怎么回事。