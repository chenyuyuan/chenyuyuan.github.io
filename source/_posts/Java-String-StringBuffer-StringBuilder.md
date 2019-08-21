---
title: Java-String-StringBuffer-StringBuilder
date: 2019-08-22 00:38:52
tags: Java
---

##### String StringBuffer StringBuilder区别

1.String是常量，而StringBuilder，StringBuffer是变量，
2.StringBuffer 与 StringBuilder 中的方法和功能完全是等价的，
3.只是StringBuffer 中的方法大都采用了 synchronized 关键字进行修饰，因此是线程安全的，而 StringBuilder 没有这个修饰，可以被认为是线程不安全的。
4.在单线程程序下，StringBuilder效率更快，因为它不需要加锁，不具备多线程安全,而StringBuffer则每次都需要判断锁，效率相对更低

https://blog.csdn.net/Alpha_Paser/article/details/89352122

##### String简析

https://www.cnblogs.com/LiaHon/p/11068050.html

##### final分析

https://www.cnblogs.com/LiaHon/p/11071861.html