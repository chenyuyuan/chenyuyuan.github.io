---
title: leetcode-155-minstack
date: 2019-07-30 00:34:57
tags: [algorithm, leetcode]
---

https://leetcode.com/problems/min-stack/

##### description:

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

<!-- more -->

Example:

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

和栈一样，但同时要求获取栈内最小值时使用O(1)的时间复杂度

##### solution:

在push时，每次得到比最小值还小的数时，将原来的最小值也一起push到stack里

在pop时，若是即将pop最小值，则pop时把原来的最小值恢复

##### code in java:

```java
class MinStack {
    int min=Integer.MAX_VALUE;
    Stack<Integer> stack=new Stack<>();
    
    /** initialize your data structure here. */
    public void push(int x) {
        if(x<=min){
            stack.push(min);
            min=x;
        }
        stack.push(x);
    }
    
    public void pop() {
        if(stack.pop() == min) min=stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        if(!stack.isEmpty())
            return min;
        else // 栈为空
            return 0;
    }
}


```

