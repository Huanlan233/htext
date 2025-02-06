+++
title = "Java 基础笔记"
date = 2025-01-17T20:19:32+08:00
draft = false
categories = ["Java"]
tags = ["Java", "Note"]
slug = "a8bed2e4"
image = "https://picsum.photos/seed/a8bed2e4/800/600"
+++

没有引言，单纯做笔记。

<!--more-->

## 运算符

### 逻辑运算符
 - `&&` 二方都为true，则为true；
 - `||` 一方为true，则为true；

### 位运算符
 - `&` 一方为0/false，则为0/false
 - `|` 一方为1/true，则为1/true

---

## String

String 在实例化完成后不可变，自带final修饰符。

### 字符串池

在JVM中有一块内存区域，专门用于存储字符串，即**字符串池（String Pool）**。

### 字面量和引用

`String xxx = new String("...")`
和
`String xxx = "..."`
不同。

 - `String xxx = new String("...")` 直接在JVM的堆（Heap）创建**全新的对象**；
 - `String xxx = "..."` 先检查池中有无相同字符串，有则**获取相同引用**。

### equals(Object)

- `equals(Object)`方法检查**字符串内容**。

- `==`检查**字符串引用**

若是用`==`，则两个字符串有可能指向同或不同引用，导致与预期结果不一致。

```Java
String a = "相同内容";
String b = new String("相同内容");

System.out.println(a == b); 
System.out.println( a.equals(b) )
```

第一个为`false`，引用不同；

第二个为`true`，引用不同，但内容相同

### intern()

`intern()` 复制一个字符串，保持引用相同。

```Java
String a = new String("Hello"); // 堆中
String b = a.intern()
String c = "Hello"; // 池中

System.out.println(a==b); // false
System.out.println(b==c); // true
```

---

## 对象地址

一些对象的地址一般以 `A@bbbbbbbb` 形式打印出来
 - A 是类的名称
 - b 是对象的哈希值

---

# 方法传参的方式

- 基本数据类型：按值
- 引用数据类型：按引用