---
title: algorithm-03-priority-queue
date: 2019-08-18 23:50:41
tags: algorithm
---

#### 问题：

1.https://leetcode.com/problems/merge-k-sorted-lists/



#### Solution:

##### 1.https://leetcode.com/problems/merge-k-sorted-lists/

code: 

```java
public class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        PriorityQueue<ListNode> queue = new PriorityQueue<ListNode>(lists.length, new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
                if (o1.val < o2.val) return -1;
                else if (o1.val == o2.val) return 0;
                else return 1;
            }
        });
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        for (ListNode node: lists)
            if (node != null)
                queue.add(node);
        while (!queue.isEmpty()) {
            tail.next = queue.poll();
            tail = tail.next;
            if (tail.next != null)
                queue.add(tail.next);
        }
        return dummy.next;
    }
}
```

