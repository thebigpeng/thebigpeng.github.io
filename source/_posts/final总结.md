---
title: final总结
date: 2021-03-07 16:23:33
tags:
 - 基础知识点
categories: Java面经基础
---

<!-- toc -->

**final**：  不可改变，最终的含义。可以用于修饰类、方法和变量。

- 类：被修饰的类，不能被继承。
- 方法：被修饰的方法，不能被重写。
- 变量：被修饰的变量，有且仅能被赋值一次。

## 1.final修饰变量

- 当final修饰的是一个<font color='cornflowerblue'>基本数据类型数据</font>时, 这个数据的值在<font color='cornflowerblue'>初始化后将不能被改变;</font> 
- 当final修饰的是一个引用<font color='cornflowerblue'>类型数据</font>时, 也就是<font color='cornflowerblue'>修饰一个对象</font>时, 引用在初始化后<font color='red'>将永远指向一个内存地址,</font> 不可修改. 但是该内存地址中保存的对象信息, 是可以进行修改的.
- 引用类型变量所指向的对象之所以可以修改, 是因为引用变量不是直接指向对象的数据, 而是指向对象的引用的. 所以被final修饰的引用类型变量将永远指向一个固定的对象, 不能被修改; 对象的数据值可以被修改.

### 1.1被final修饰的常量在编译阶段会被放入常量池中

```java
public class demo {
    public static void main(String[] args) {
       String str = "20192109";
       final String str2 = "2019";
       int aa = 2019;

       String str3 = str2 + "2109";
       String str4 = aa + "2019";

       System.out.println(str == str3);
       System.out.println(str == str4);

    }
}
```

输出

```java
true
false
```

原因分析：final是用于定义常量的, 定义常量的好处是: 不需要重复地创建相同的变量. 而常量池是Java的一项重要技术, 由final修饰的变量会在编译阶段放入到调用类的常量池中.

>  整数-127-128是默认加载到常量池里的, 也就是说如果涉及到-127-128的整数操作, 默认在编译期就能确定整数的值. 所以这里我故意选用数字2019(大于128), 避免数字默认就存在常量池中.

上面的代码运作过程是这样的:

- 首先根据final修饰的常量会在编译期放到常量池的原则, n2会在编译期间放到常量池中.

- 然后s变量所对应的"20192109"字符串会放入到字符串常量池中, 并对外提供一个引用返回给s变量.

- 这时候拼接字符串str4, 由于aa对应的数据没有放入常量池中, 所以str4暂时无法拼接, 需要等程序加载运行时才能确定s1对应的值.

- 但在拼接str3的时候, 由于str2已经存在于常量池, 所以可以直接与"2109"拼接, 拼接出的结果是"20192109". 这时系统会查看字符串常量池, 发现已经存在字符串20192109, 所以直接返回20192109的引用. 所以str和str2指向的是同一个引用, 这个引用指向的是字符串常量池中的20192109.

- 当程序执行时, aa变量才有具体的指向.

- 当拼接str4的时候, 会创建一个新的String类型对象, 也就是说字符串常量池中的20192109会对外提供一个新的引用.

- 所以当s1与s用"=="判断时, 由于对应的引用不同, 会返回false. 而s2和s指向同一个引用, 返回true.

链接：https://juejin.cn/post/6844903849040281614
案例来源：掘金

## 2.final修饰方法

修饰方法有两个作用：

- <font color='orange'>锁定方法, 不让任何继承类对其进行修改</font>.
- 在编译器对方法进行<font color='cornflowerblue'>内联,</font> 提升效率. (现在已经基本不用，原理如下：当调用一个方法时, 系统需要进行保存现场信息, 建立栈帧, 恢复线程等操作, 这些操作都是相对比较耗时的. 如果使用final修饰一个了一个方法a, 在其他调用方法a的类进行编译时, 方法a的代码会直接嵌入到调用a的代码块中.)

## 3.final修饰类

- 使用final修饰类的目的简单明确: 表明这个类不能被继承.
- 当程序中有永远不会被继承的类时, 可以使用final关键字修饰
- 被final修饰的类所有成员方法都将被隐式修饰为final方法.

## 4.好处

- 使用final关键字提高了性能，JVM和java应用会缓存final变量
- final变量在多线程下是**线程安全的**
- 使用final关键字提高了性能的原因是JVM会对被final修饰的变量、类和方法进行优化。