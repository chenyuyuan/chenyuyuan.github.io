---
title: leetcode-1115-print-foobar-alternately
date: 2019-07-17 09:17:34
tags: [leetcode, concurrency]
---

https://leetcode.com/problems/print-foobar-alternately/

description:

Suppose you are given the following code:

```java
class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
```

The same instance of `FooBar` will be passed to two different threads. Thread A will call `foo()` while thread B will call `bar()`. Modify the given program to output "foobar" *n* times.

<!-- more -->

Example:

```
Input: n = 1
Output: "foobar"
Explanation: There are two threads being fired asynchronously. One of them calls foo(), while the other calls bar(). "foobar" is being output 1 time.
```

```
Input: n = 2
Output: "foobarfoobar"
Explanation: "foobar" is being output 2 times.
```

#### solution:

use semaphore to restrict the order of two threads.

#### code in java:

```java
import java.util.concurrent.Semaphore;
class FooBar {
    private int n;
    Semaphore s1 = new Semaphore(0);
    Semaphore s2 = new Semaphore(1);
    public FooBar(int n) {
        this.n = n;
    }
    public void foo(Runnable printFoo) throws InterruptedException {

        for (int i = 0; i < n; i++) {
            s2.acquire();
            printFoo.run();
            s1.release();
        }
    }
    public void bar(Runnable printBar) throws InterruptedException {

        for (int i = 0; i < n; i++) {
            s1.acquire();
            printBar.run();
            s2.release();
        }
    }
}
```

