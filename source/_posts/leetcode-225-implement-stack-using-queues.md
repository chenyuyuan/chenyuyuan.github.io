---
title: leetcode-225-implement-stack-using-queues
date: 2019-07-30 13:14:03
tags: leetcode
---

https://leetcode.com/problems/implement-stack-using-queues/

##### code in java:

```java
class MyStack {

    public LinkedList<Integer> queue;

    /** Initialize your data structure here. */
    public MyStack() {
        this.queue = new LinkedList<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        queue.add(x);
        for(int i = 0;i < queue.size() - 1;i++) {
            queue.add(queue.poll());
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.poll();
    }

    /** Get the top element. */
    public int top() {
        return queue.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }

}
```

<!-- more -->