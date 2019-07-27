---
title: spto-06-print-list-reversingly
date: 2019-07-25 19:47:27
tags: spto
---

### 面试题6: 

##### 题目: 

输入一个链表的头节点，从头到尾反过来打印出每个节点的值，链表节点的定义如下: 

```java
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int x) { val = x; }
}
```

<!-- more -->

##### solution: 

可以使用递归的方法，但是当链表很长的时候，可能会导致函数调用栈溢出。

也可以使用栈基于循环实现的代码，显然这种方法鲁棒性更好。

##### code in java: 

```java
public class question06 {
    public static void main (String[] args) {
        ListNode p1 = new ListNode(1);
        ListNode p2 = new ListNode(2);
        ListNode p3 = new ListNode(3);
        p1.next = p2;
        p2.next = p3;
        System.out.println("printListReversingly_Iterativity() is below: ");
        printListReversingly_Recursively(p1);
        System.out.println("printListReversingly_Recursively() is below: ");
        printListReversingly_Recursively(p1);
    }
    public static void printListReversingly_Iterativity(ListNode head) {
        Stack<ListNode> stack = new Stack<>();
        ListNode p = head;
        while (p != null) {
            stack.push(p);
            p=p.next;
        }
        while (!stack.isEmpty()) {
            System.out.print(stack.pop().val + " ");
        }
    }
    public static void printListReversingly_Recursively(ListNode head) {
        if(head != null) {
            if(head.next != null) {
                printListReversingly_Recursively(head.next);
            }
            System.out.println(head.val + " ");
        }
    }
}


```

