---
title: java设计模式
date: 2019-03-23 13:51:29
tags: [java,面对对象编程]
---

### 这是啥
> 设计模式（Design Pattern）是前辈们对代码开发经验的总结，是解决特定问题的一系列套路。它不是语法规定，而是一套用来提高代码可复用性、可维护性、可读性、稳健性以及安全性的解决方案。讲道理这个还是挺重要的。反正我看自己写的代码，感觉挺丑的。
> 四人帮提出的一个思想，是面对对象中常用的设计原则，关键的思想在于
+ 对接口编程而不是对实现编程。
+ 优先使用对象组合而不是继承。

### 分类
+ 创建型模式
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。
+ 结构型模式
这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。
+ 行为型模型
这些设计模式特别关注对象之间的通信。
+ J2EE 模式
这些设计模式特别关注表示层。这些模式是由 Sun Java Center 鉴定的。

### 6大原则
+ 开闭原则
对扩展开放，对修改关闭。

+ 里氏代换原则（Liskov Substitution Principle）
任何基类可以出现的地方，子类一定可以出现

+ 依赖倒转原则
针对接口编程，依赖于抽象而不依赖于具体

+ 接口隔离原则
使用多个隔离的接口，比使用单个接口要好。目的在于降低类之间的耦合度

+ 迪米特法则，最少知道原则
一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立

+ 合成复用原则
尽量使用合成/聚合的方式，而不是使用继承

> emememem 可以说很复杂了

### 工厂模式
+ 创建型模式
+ 目的：**定义一个创建对象的接口**，让子类自己去决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。
+ 个人看法，这个好像就是说，做一个接口去实现对象的创建，目的有啥呢，一个是扩展性好，加产品加一个工厂，加一个实现类就完事了[额，感觉到后面会越来越多]，还有就是封装性？还有就是，他说是对一些比较复杂的对象有好处。

### 抽象工厂模式
+ 是围绕一个超级工厂创建其他工厂
+ 目的：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类
+ 个人看法，不是说对扩展开放，对修改关闭，问题这里，没有办法解决把，增加一个类，则要增加一个类，也要加一个工厂，但是这里的抽象工厂也是要进行修改才行。不过也只是要修改那个工厂制作的东西

### 单例模式
+ 只能有一个实例，并且只能自己去创建自己的实例，单例类必须给所有其他对象提供这一实例
+ 目的在于解决一个全局使用得类频繁的创建与销毁
+ 几种实现的方式，目的在于一个安全性的考虑

#### 懒汉式模式
+ lazy初始化
+ 多线程不安全
```java
public class xianchen
{
    private static xianchen instance;
    private xianchen(){}
    public static xianchen getinstance()
    {
        if(instance == null)
            instance = new xianchen();
        return instance;
    }
}
```
#### 懒汉式，线程安全
+ lazy初始化
+ 多线程安全(加了一个synchronized锁)
> 这个synchronized是java的一种同步锁，这里旧的说同步是啥，线程同步主要的目的是防止不同的线程对同一个文件同时进行操作造成奇奇怪怪的操作。然后这里又涉及到了进程与线程，还有java的多线程，哇，好菜啊，啥都不会。
```java
public class xianchen
{
    private static xianchen instance;
    private xianchen(){}
    public static synchronized xianchen getinstance()
    {
        if(instance == null)
            instance = new xianchen();
        return instance;
    }
}
```
#### 饿汉式，线程安全
+ 没lazy初始化
+ 它基于 classloader 机制避免了多线程的同步问题，
```java
public class xianchen
{
    private static xianchen instance = new xianchen();
    private xianchen(){}
    public static xianchen getInstance()
    {
        return instance;
    }
}
```
#### 双检锁/双重校验锁
+ lazy初始化
+ 多线程安全
+ volatile目的在于防止编译器优化，编译器在每次用到这个变量时会小心的**重新**读取这个变量的值，而不是使用保存在寄存器的备份。
```java
public class xianchen
{
    private volatile static xianchen instance;
    private xianchen(){}
    public static xianchen getXianchen()
    {
        if(instance == null )
        {
            synchronized(instance.class)
            {
                if(instance == null)
                {
                    instance = new xianchen();
                }
            }
        }
        return instance
    }
}
```
#### 登记式/静态内部类
+ lazy初始化
+ 多线程安全(classloader)
+ 目的在于延迟加载实例，**只有显式调用才能出现显式装载**
```java
public class xianchen
{
    private static class xianchenHolder{
        private static final xianchen instance = new xianchen()
    }
    private xianchen(){}
    public static final xianchen getInstance 
    {
        return xianchenHolder.instance;
    }
}
```
#### 枚举
+ 好象还没有普及使用



### 建造者模式
> 使用多个简单的对象一步一步构建成一个复杂的对象，这种类型的设计模式属于创建型模式，**提供了一种创建对象**
> 目的在于解决软件系统中，面临的“一个复杂对象”的创建工作，由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是组合他们的算法却相对稳定。