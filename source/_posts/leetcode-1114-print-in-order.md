---
title: leetcode-1114-print-in-order
date: 2019-07-16 12:16:44
tags: leetcode
---

### leetcode 1114. print in order

https://leetcode.com/problems/print-in-order/

description

Suppose we have a class:

```
public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
```

The same instance of `Foo` will be passed to three different threads. Thread A will call `first()`, thread B will call `second()`, and thread C will call `third()`. Design a mechanism and modify the program to ensure that `second()` is executed after `first()`, and `third()` is executed after `second()`.

**Example 1:**

```
Input: [1,2,3]
Output: "firstsecondthird"
Explanation: There are three threads being fired asynchronously. The input [1,2,3] means thread A calls first(), thread B calls second(), and thread C calls third(). "firstsecondthird" is the correct output.
```

**Example 2:**

```
Input: [1,3,2]
Output: "firstsecondthird"
Explanation: The input [1,3,2] means thread A calls first(), thread B calls third(), and thread C calls second(). "firstsecondthird" is the correct output.
```

**Note:**

We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seems to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.

#### method1: 

use semaphore

code: (java solution)

```java
import java.util.concurrent.*;
class Foo {
    Semaphore run2, run3;
    public Foo() {
        run2 = new Semaphore(0);
        run3 = new Semaphore(0);
    }

    public void first(Runnable printFirst) throws InterruptedException {
        printFirst.run();
        run2.release();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        run2.acquire();
        printSecond.run();
        run3.release();
    }
    public void third(Runnable printThird) throws InterruptedException {
        run3.acquire(); 
        printThird.run();
    }
}
```

my solution: 

use a flag

code: (java solution)

```java
class Foo {
    int flag = 0;
    public Foo() {
    }

    public void first(Runnable printFirst) throws InterruptedException {

        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        flag = 1;
    }

    public void second(Runnable printSecond) throws InterruptedException {

        // printSecond.run() outputs "second". Do not change or remove this line.
        while(flag < 1){
            Thread.sleep(1);
        }

        if(flag == 1){
            printSecond.run();
            flag = 2;
        }

    }

    public void third(Runnable printThird) throws InterruptedException {

        // printThird.run() outputs "third". Do not change or remove this line.
        while(flag < 2){
            Thread.sleep(1);
        }
        if(flag == 2){
            printThird.run();
        }
    }
}
```

