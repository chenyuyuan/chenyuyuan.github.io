---
title: algorithm-01-array-and-linked-list
date: 2019-08-16 21:43:02
tags: algorithm
---

#### 题目：

1.https://leetcode.com/problems/reverse-linked-list/

2.https://leetcode.com/problems/swap-nodes-in-pairs/

3.https://leetcode.com/problems/linked-list-cycle/

4.https://leetcode.com/problems/linked-list-cycle-ii/

5.https://leetcode.com/problems/reverse-nodes-in-k-group/

6.https://leetcode.com/problems/intersection-of-two-linked-lists/ (两支链表都没有环，那么有环的情况该怎么考虑呢？)

```java
// Definition for singly-linked list.
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```



#### Solution: 

##### 1.https://leetcode.com/problems/reverse-linked-list/

code:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode p = null;
        ListNode q = head, r = head;
        while (q != null) {
            r = r.next;
            q.next = p;
            p = q;
            q = r;
        }
        return p;
    }
}
```

##### 2.https://leetcode.com/problems/swap-nodes-in-pairs/

code:

用迭代方法：

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode firstPointer = new ListNode(0);
        ListNode pre = firstPointer;
        pre.next = head;
        while(pre.next != null && pre.next.next != null) {
            ListNode a = pre.next;
            ListNode b = pre.next.next;
            a.next = b.next;
            pre.next = b;
            b.next = a;
            pre = a;
        }
        return firstPointer.next;
    }
}
```

递归方法：

```java
public class Solution {
    public ListNode swapPairs(ListNode head) {
        if ((head == null)||(head.next == null))
            return head; // 包括返回null的情况和只有一个节点直接返回head的情况
        ListNode n = head.next; // 两个节点中右边那个点
        head.next = swapPairs(head.next.next); // head的next会连接下一组的n
        n.next = head; // 右边节点指向左边节点
        return n;
    }
}
```

##### 3.https://leetcode.com/problems/linked-list-cycle/

code:

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (slow != null && fast != null && fast.next != null) { // 这里的fast.next != null一定要有，否则如果fast.next == null，下面的fast = fast.next.next;会出现Runtime Error
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```

##### 4.https://leetcode.com/problems/linked-list-cycle-ii/

fast指针与slow指针相遇的位置到环入口的距离，与起点到环入口的距离相等（为什么？）

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast!=null && fast.next!=null){
            fast = fast.next.next;
            slow = slow.next;

            if (fast == slow){
                fast = head; 
                while (fast != slow){
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```

##### 5.https://leetcode.com/problems/reverse-nodes-in-k-group/



##### 6.https://leetcode.com/problems/intersection-of-two-linked-lists/

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;
        ListNode a = headA;
        ListNode b = headB;
        while (a != b) {
            a = a == null ? headB : a.next;
            b = b == null ? headA : b.next;
        }
        return a;
    }
}
```

