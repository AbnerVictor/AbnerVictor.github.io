---
layout: post
title: 'Java 学习笔记'
subtitle: '从到放弃'
date: 2019-09-11
categories: HKUST
tags:  Java
---

[toc]

## COMP 3021 - Java Programming

***

[Course Page](https://course.cse.ust.hk/comp3021/)

## Self-learning

***

### 起步

1. 一个简单的实例

```Java
public class HelloWorld{
public static void main(String[] args){
System.out.println(“Helloworld”);
}

``

> **注：** `String args[]` 与 `String[] args` 都可以执行，但推荐使用后者，避免歧义

Java 编译指令

```shell
$ javac HelloWorld.java
$ java HelloWorld
Hello World
```

### Java基础语法

- Java是大小写敏感的；

- 类名的首字母应该大写，如：`MyFirstJavaClass`；

- 所有的函数名应该以小写字母开头，如果方法名包含若干单词，则后面的每个单词首字母大写；

- 源文件名必须和类名相同；

- 主方法入口：所有的Java程序由

`public static void main(String[] args)`方法开始执行

#### Java标识符

Java中，类名、变量名和函数名被称为标识符。

1. 标识符应该以字母、美元符号`$`或者是下划线`_`开始；

2. 标识符中可以含有数字（首字符除外）；

3. 关键字不能用作标识符，标识符大小写敏感；

#### Java修饰符

- 访问控制修饰符：default，public，protected，private

- 非访问控制修饰符：final，abstract，static，synchronized

#### Java变量

- 局部变量

- 类变量（静态）

- 成员变量（非静态）

#### Java枚举

```java

class FreshJuice{

enum FreshJuiceSize{ SMALL, MEDIUM, LARGE }//枚举

FreshJuiceSize size;
}	public class FreshJuiceTest{
public static void main(String[] args){
FreshJuice juice = new FreshJuice();
juice.size = FreshJuiceSize.FreshJuiceSize.MEDIUM;
}
}
```

**HINT**：枚举可以单独声明也可以声明在类里面。方法、变量、构造函数也可以在枚举中定义。

#### Java关键字

[关键字对照表](http://www.runoob.com/java/java-basic-syntax.html)

#### Java继承

在 Java 中，一个类可以由其他类派生。如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。

利用继承的方法，可以重用已存在类的方法和属性，而不用重写这些代码。被继承的类称为超类（super class），派生类称为子类（subclass）。

#### 接口

关键字：`interface`，类似于c++中的`virtual`虚函数。

接口只定义方法，实现取决于派生类，实现虚函数的关键字`implement`

### Java对象和类

- 对象 object：类的一个实例

- 类 class：描述一类对象的行为和状态

类有若干种访问级别，也分不同的类型：抽象类和final类等，在访问控制中介绍。

Java还有一些特殊的类，比如内部类、匿名类。

#### 变量

- 局部变量：在函数、构造函数中定义的变量，变量的生命和初始化都在函数中，函数结束后，变量就会自动销毁；

- 成员变量：定义在类中，函数之外的变量，在创建对象的时候被实例化；

- 类变量：类变量也在类中声明，在函数之外，但必须声明为static类型

#### 构造函数

如果没有显式定义类的构造函数，Java编译器会提供一个默认构造函数。

一个类可以有多个构造函数，在创建一个对象的时候至少调用其中一个。

```java
public class Puppy{
	public Puppy(){}
	public Puppy(String name){}
}
```

#### 创建对象

在Java中，使用关键字new来创建一个新的对象：

1. 声明一个对象，包括对象的名称和类型

2. 实例化：使用关键字new来创建一个对象

3. 初始化：使用new创建对象时，调用构造方法初始化对象

```java

public class Puppy{
	public Puppy(String name){}
	public static void main(String[] args){
		Puppy myPuppy = new Puppy("dog");
	}
}

```

#### 源文件声明规则

- 一个源文件中只能有一个public类

- 一个源文件中可以有多个非private类

- 源文件的名称应该和public类名保持一致

- 如果一个类定义在某个包中，那么package语句应该在源文件首行

- 如果源文件包含import语句，那么应该放在package语句和类定义之间

- import语句和package语句对源文件中定义的所有类都有效，在同一个源文件中，不能给不同的类不同的包声明
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjIxNTU3ODYzLDE2ODg0ODE0NywtMzkyMD
A2OTc5LC0xMjgxMDg3NTYxXX0=
-->