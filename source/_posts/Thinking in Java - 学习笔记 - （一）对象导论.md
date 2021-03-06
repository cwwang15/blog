---
title: Thinking in Java - 学习笔记 - （一）对象导论
date: 2018-04-30 15:39:08
categories:
	- 学习笔记
tags: 
	- Thinking in Java
	- Java
---

面向对象程序设计方式
-----------------------------
1. **万物皆为对象**
2. **程序是对象的集合，它们通过发送消息来告知彼此所要做的**
3. **每个对象者有自己的由其他对象所构成的存储**
4. **每个对象都拥有其类型**
5. **某一特定类型的所有对象都可以接收同样的消息**

<!--more-->

<q>*Booch 对对象提出了一个更加简洁的描述：对象具有状态、行为和标识。*</q>

隐藏实现细节
-----------------
### 访问控制

1. 让客户端程序员无法触及他们不应该触及的部分。
2. 允许库设计者可以改变类内部的工作方式而不用担心会影响到客户端程序员。

### 访问指定词 - access specifier

**public, private, protected**

<q>*当不使用访问指定词时，默认访问权限--包访问权限--将发挥作用。*</q>


复用具体实现
-----------------
有两种方式：

- 组合（composttion）：使用现有的类合成新的类，“has-a” 关系
- 聚合（aggregation）：动态发生的组合


继承
------

### 关于继承的结果
- 导出类无法访问基类的private成员
- 导出类复制了基类的接口，所有发送给基类对象的消息同时也可以发送给导出类对象。也就意味着导出类与基类具有相同的类型。


有两种方法可以使基类与导出类产生差异

1. 直接在导出类中添加新方法：*is-like-a*（像是一个）关系
2. 覆盖（overriding）：*is-a*（是一个）关系



多态
------

在处理类型的层次结构时，经常想把一个对象不当作它所属的特定类型来对待，而是将其当作其基类的对象来对待。

但是，在试图将导出类型的对象当作其泛化基类型对象来看待时，仍然存在一个问题。如果某个方法要让泛化几何形状绘制自己、让泛化交通工具行驶，那么编译器是不可能知道应该执行哪一段代码的。

编译器不可能产生传统意义上的函数调用。

<q>*一个非面向对象编程的编译器产生的函数调用会引起所谓的 <font face="楷体">前期绑定</font>，这么做意味着编译器将产生对一个具体函数名字的调用，而运行时将这个调用解析到将要被执行的代码的绝对地址。*</q>

为解决这个问题，面向对象程序设计语言使用了 <font face="楷体">后期绑定</font> 的概念。当向对象发送消息时，被调用的代码直到运行时才能确定。

- 为了执行后期绑定，Java 使用一小段特殊的代码来替代绝对地址调用。
- 在 Java 中，动态绑定是默认行为，不需要添加额外的关键字来实现多态。

### 一个例子
``` java
    void doSomething(Shape shape) {
        shape.erase();
        // ...
        shape.draw();
    }

```

```
    Circle circle = new Circle();
    doSomething(circle);
```
Circle 被传入到预期接收 Shape 的方法中，由于 Circle 可以被 doSomething() 看作是 Shape，所以这样做是完全安全且合乎逻辑的。

把将导出类看做是它的基类的过程称为 <font face="楷体">向上转型</font>(upcasting)。


单根继承结构
------------------

在 Java 中，所有的类最终都继承自单一的基类，这个终极基类的名字就是 Object。

- 在单继承结构中的所有对象都具有一个共用接口，所以它们归根到底都是相同的基本类型。
- 单根继承结构保证所有对象都具备某些功能。
- 单根继承结构使垃圾回收器的实现变得容易得多。


容器
------
如何才能知道需要多少空间来创建这些对象呢？答案是不可能知道，因为这类信息只有在运行时才能获得。
面向对象设计的解决方案：创建另一种对象类型。这种新的对象类型持有对其他对象的引用。（通常被称为容器）

### 参数化类型
参数化类型机制：创建这样的容器，它知道自己所保存的对象的类型，从而不需要向下转型以及消除犯错误的可能。（在 Java 中称为范型）
``` java
ArrayList<Shape> shapes = new ArrayList<Shape>();
```


对象的创建和生命期
---------------------------
在使用对象时，最关键的问题之一便是它们的生成和销毁方式。每个对象为了生存都需要资源，尤其是内存。当我们不再需要一个对象时，它必须被清理掉，使其占有的资源可以被释放和重用。

**如何控制对象的生命周期？**

- C++ 认为效率控制是最重要的议题，所以给程序员供了选择的权力为也追求最大的执行速度，对象的存储空间和生命周期可以在编写程序是确定，这可以通过将对象置于堆栈（它们有时被称为自动变量（automatic varivable）或限域变量（scoped variable））或静态存储区域内来实现。

- 在被称为 **堆**（heap）的内存池中动态地创建对象。在这种方式中，直到运行时才知道需要多少对象，它们的生命周期如何，以及它们的具体类型是什么。这些问题的答案只能在程序运行时相关代码被执行到的那一刻才能确定。


异常处理：处理错误
---------------------------

- 异常不能被忽略，所以它保证一定会在某处得到处理。
- 异常提供了一种从错误状况进行可靠恢复的途径。
- 异常处理不是面向对象的特征——尽管在面向对象语言中异常常被表示成为一个对象。异常处理在面向对象语言出现之前就已经存在了。


并发编程
------------
在计算机编程中有一个基本概念，就是在同一时刻处理多个任务的思想。
把问题切分成多个可独立运行的部分（任务），从而提高程序的响应能力。在程序中，这些彼此独立运行的部分称之为线程，上述概念被称为“并发”。

并发的隐患：共享资源。

Java 与 Internet
----------------------

术语
CGI: common gateway interface
GUI: graphic user interface

