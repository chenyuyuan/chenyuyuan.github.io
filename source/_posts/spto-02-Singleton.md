---
title: spto-02 Singleton
date: 2019-07-12 23:24:49
tags: [book,剑指offer]
categories: spto
---

问题描述：实现Singleton模式，设计一个类，只能生成该类的一个实例。
<!-- more -->

有三种比较好的实现方式

1. 饿汉式，是线程安全的，使用final

   饿汉式：在程序启动或单件模式类被加载的时候，单件模式实例就已经被创建。

   ```java
   public class Singleton {
       private final static Singleton INSTANCE = new Singleton();
       private Singleton(){
           
       }    
       public static Singleton getInstance() {        
           return INSTANCE;    
       }
   }
   ```

2. 使用内部静态类，线程安全

   final修饰类，不能被继承

   final修饰方法，锁定方法，防止继承类修改它的含义，(另一为了效率)

   

   ```java
   class Singleton4 {
   	private final static class SingletonHolder {
       	private static final Singleton4 INSTANCE = new Singleton4();    
       }    
       private Singleton4() {    
           
       }    
       public static Singleton4 getInstance() {        
           return SingletonHolder.INSTANCE;    
       }
   }
   ```

3. 懒汉式, 使用双重校验锁, 线程安全

   懒汉式：当程序第一次访问单件模式实例时才进行创建。

   ```java
   class Singleton6 {    
       private volatile static Singleton6 instance = null;    
       private Singleton6() {    
       
       }    
       public static Singleton6 getInstance() {        
           if(instance == null) {            
               synchronized (Singleton6.class) {                
                   if(instance == null) {                    
                       instance = new Singleton6();                
                   }            
               }        
           }        
           return instance;    
       }
   }
   ```