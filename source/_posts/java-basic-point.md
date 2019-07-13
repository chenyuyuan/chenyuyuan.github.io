---
title: java basic point
date: 2019-07-10 20:53:38
tags: Java
categories: Java
---

### How Java Run?

source code -> using Java compiler ->compile the source code to byte code(.class file) 
<!-- more -->
-> ||------------------------------------------------------||       <-(this is JRE)

​     || -> 类装载器 ->字节码检验器->解释器     ||    ->系统平台

​      ||-----------------------------------------------------||

### What is JDK, JRE, JVM?  

JVM: Java虚拟机, 虚拟用于执行字节码的虚拟计算机

JRE: Java运行时环境，有JVM，库函数，运行Java运行时环境所必须的文件

JDK: 有JRE+编译器和调试器，用于开发

