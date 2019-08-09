---
title: Binary-tree-treversal
date: 2019-07-26 16:02:00
tags: algorithm
---

### 二叉树的遍历

先序遍历，中序遍历，后序遍历

先序遍历(非递归): 

```java
public List<Integer> preorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    LinkedList<Integer> ll = new LinkedList<>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            ll.add(p.val);
            p = p.left;
        }
        else {
            p = stack.pop();
            p = p.right;
        }
    }
    return ll;
}
```

<!-- more -->

中序遍历(非递归): 

```java
public List<Integer> inorderTraversal(TreeNode root) {
    LinkedList<Integer> list=new LinkedList<>();
    Stack<TreeNode> stack=new Stack<>();
    TreeNode p=root;
    while(!stack.isEmpty()||p!=null){
        if(p!=null){
            stack.push(p);
            p=p.left;
        }
        else{
            TreeNode node=stack.pop();
            list.add(node.val);
            p=node.right;
        }
    }
    return list;
}
```

后序遍历(非递归): 

```java
public List<Integer> postorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    LinkedList<Integer> ll = new LinkedList<>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            ll.addFirst(p.val);
            p=p.right;
        }
        else {
            p = stack.pop();
            p = p.left;
        }
    }
    return ll;
}
```

