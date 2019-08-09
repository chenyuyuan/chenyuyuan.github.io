---
title: Java-import-static
date: 2019-08-09 16:31:03
tags: Java
---

##### import与import static的区别

import

1.导入某个包中的任何一个声明为public的类或者接口

2.仅导入声明目录下面的类或者接口而不导入子包下的

3.默认会导入java.lang包下的

import static 从JDK1.5开始(导入后可直接调用相应的静态方法或者属性)

1.导入类下的静态方法或者静态属性     (范围修饰符不能为private，否则会报错)

2.写法为导入某个类下的所有静态变量或者方法:import static com.utils.ClassName.*;导入某个类下的某个静态方法或者变量:import static com.utils.ClassName.methodOrVarName;

##### import static官方文档的解释

In order to access static members, it is necessary to qualify references with the class they came from. For example, one must say:

> ```java
> double r = Math.cos(Math.PI * theta);
> ```

In order to get around this, people sometimes put static members into an interface and inherit from that interface. This is a bad idea. In fact, it's such a bad idea that there's a name for it: the *Constant Interface Antipattern*[*Effective Java*](http://java.sun.com/docs/books/effective/)

The static import construct allows unqualified access to static members *without* inheriting from the type containing the static members. Instead, the program *imports* the members, either individually:

> ```java
> import static java.lang.Math.PI;
> ```

or en masse:

> ```java
> import static java.lang.Math.*;
> ```

Once the static members have been imported, they may be used without qualification:

> ```java
> double r = cos(PI * theta);
> ```

The static import declaration is analogous to the normal import declaration. Where the normal import declaration imports classes from packages, allowing them to be used without package qualification, the static import declaration imports static members from classes, allowing them to be used without class qualification.

So when should you use static import? **Very sparingly!** Only use it when you'd otherwise be tempted to declare local copies of constants, or to abuse inheritance (the Constant Interface Antipattern). In other words, use it when you require frequent access to static members from one or two classes. If you overuse the static import feature, it can make your program unreadable and unmaintainable, polluting its namespace with all the static members you import. Readers of your code (including you, a few months after you wrote it) will not know which class a static member comes from. Importing *all* of the static members from a class can be particularly harmful to readability; if you need only one or two members, import them individually. Used appropriately, static import can make your program *more* readable, by removing the boilerplate of repetition of class names.

##### 翻译

为了访问静态成员，有必要使用它们来自的类来限定引用。例如，必须说：

> ```java
> double r = 数学。cos（Math.PI * theta）;
> ```

为了解决这个问题，人们有时会将静态成员放入接口并从该接口继承。这是一个坏主意。事实上，它有一个名称是一个坏主意：*Constant Interface Antipattern*[*Effective Java*](http://java.sun.com/docs/books/effective/)

静态导入构造允许对静态成员进行非限定访问， *而不*从包含静态成员的类型继承。相反，程序会单独*导入*成员：

> ```java
> import static java.lang.Math.PI;
> ```

或集体：

> ```java
> import static java.lang.Math。*;
> ```

导入静态成员后，可以无限制地使用它们：

> ```java
> double r = cos（PI * theta）;
> ```

静态导入声明类似于正常的导入声明。如果正常的导入声明从包中导入类，允许它们在没有包限定的情况下使用，静态导入声明从类中导入静态成员，允许它们在没有类限定的情况下使用。

那么什么时候应该使用静态导入？ **非常谨慎！**只有在您试图声明常量的本地副本或滥用继承（Constant Interface Antipattern）时才使用它。换句话说，当您需要频繁访问来自一个或两个类的静态成员时，请使用它。如果过度使用静态导入功能，它可能会使您的程序不可读且无法维护，并使用您导入的所有静态成员污染其命名空间。您的代码的读者（包括您，在您编写它几个月后）将不知道静态成员来自哪个类。*全部*导入一个类中的静态成员对可读性特别有害; 如果您只需要一个或两个成员，请单独导入它们。如果使用得当，静态导入可以通过删除重复类名的样板来使程序*更具*可读性。