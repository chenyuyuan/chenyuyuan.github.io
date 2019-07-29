---
title: leetcode-232-implement-queue-using-stacks
date: 2019-07-30 01:04:53
tags: leetcode
---

https://leetcode.com/problems/implement-queue-using-stacks/

##### description:

用栈实现队列

##### code in java: 

```java
class MyQueue {
    public Stack<Integer> stack1 = new Stack<>();
    public Stack<Integer> stack2 = new Stack<>();

    /** Initialize your data structure here. */
    public MyQueue() {

    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        stack1.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (stack2.size() <= 0) {
            while (!stack1.isEmpty()) {
                int temp = stack1.pop();
                stack2.push(temp);
            }
        }
        return stack2.pop();
    }

    /** Get the front element. */
    public int peek() {
        if (stack2.size() <= 0) {
            while (!stack1.isEmpty()) {
                int temp = stack1.pop();
                stack2.push(temp);
            }
        }
        return stack2.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        if(stack1.isEmpty() && stack2.isEmpty()) 
            return true;
        return false;
    }

}
```

