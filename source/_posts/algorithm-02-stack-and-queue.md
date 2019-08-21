---
title: algorithm-02-stack-and-queue
date: 2019-08-17 22:34:11
tags: algorithm
---

#### 问题：

1.https://leetcode.com/problems/valid-parentheses

2.https://leetcode.com/problems/implement-queue-using-stacks

3.https://leetcode.com/problems/implement-stack-using-queues

4.https://leetcode.com/problems/backspace-string-compare

#### Solution:

##### 1.https://leetcode.com/problems/valid-parentheses

code:

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(')
                stack.push(')');
            else if (c == '{')
                stack.push('}');
            else if (c == '[')
                stack.push(']');
            else if (stack.isEmpty() || stack.pop() != c)
                return false;
        }
        return stack.isEmpty();
    }
}
```

##### 2.https://leetcode.com/problems/implement-queue-using-stacks

使用俩栈stack1，stack2

当push时直接push到stack1

当pop时若stack2不为空则直接从stack2 pop，当pop为空时先把stack1的元素全部倒腾到stack2，然后再pop stack2

code:

完整代码：https://leetcode.com/submissions/detail/247284493/

##### 3.https://leetcode.com/problems/implement-stack-using-queues

Java可以用Queue的LinkedList

用一个queue，push时，先将元素add进queue，然后将queue原有的元素一个个按顺序重新放queue

这样的话就能一直保持stack的pop顺序

其他操作不变

code:

完整代码：https://leetcode.com/submissions/detail/252412117/



code:

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        return res(S).equals(res(T));
    }
    private String res(String s){
        char[] ca = s.toCharArray();
        Stack<Character> stack = new Stack<>();
        for(char c: ca){
            if(c != '#'){
                stack.push(c);
            }
            else if(!stack.empty()){
                stack.pop();
            }
        }
        return String.valueOf(stack);
    }
}
```

