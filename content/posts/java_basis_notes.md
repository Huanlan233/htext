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

---

## 运算符

### 逻辑运算符
 - `&&` 二方都为true，则为true；
 - `||` 一方为true，则为true；

### 位运算符
 - `&` 一方为0/false，则为0/false
 - `|` 一方为1/true，则为1/true

## String

String 在实例化完成后不可变，自带`final`修饰符。

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

`intern()` 复制一个字符串，保持**在池中**引用相同。

```Java
String a = new String("Hello"); // 堆中
String b = a.intern()
String c = "Hello"; // 池中

System.out.println(a==b); // false
System.out.println(c==b); // true
```

## 对象地址

一些对象的地址一般以 `A@bbbbbbbb` 形式打印出来
 - A 是类的名称
 - b 是对象的哈希值

## 方法传参的方式

- 基本数据类型：按值
- 引用数据类型：按引用

## 静态代码块

写法如下

```Java
class A {
    static {
        // 静态代码块
    }
}
```

随着类的加载而运行，且只运行一次。

## 内部类
### 成员内部类

JDK 16 之前不能在成员内部类内定义静态字段。

```java
class Outer {
    // 其他代码
    class Inner {
        // 内部类
    }
} 
```

创建方法一般如下：
```java
Outer.Inner inner = new Outer().new Inner();
```

在内部类的字节码文件内，有一个 `Outer` 类型的隐藏变量 `Outer.this`，指代外部类

因此，可以通过内部类来访问外部类的私密字段：
```java
public class Outer {
    private final String id; // 此处为 private

    public Outer(String id) {
        this.id = id;
    }

    class Inner {
        public void printId() {
            // 可以通过 外部类.this.XXX 方式访问字段，方法等。
            System.out.println(Outer.this.id); 
            
        }
    }
}

public static void main(String[] args) {
        Outer.Inner inner = new Outer("id1").new Inner();

        inner.printId(); // 输出 "id1"
}
```

### 静态内部类

顾名思义:

```java
class Outer {
    static class Inner {
        // 代码
    }
}
```

不需要先创建外部类，可以直接创建。

```java
Outer.Inner inner = new Outer.Inner();
```

也许有点抽象，但是静态内部类并太类似静态字段。

```java
Outer.Inner inner1 = new Outer.Inner();
Outer.Inner inner2 = new Outer.Inner();

System.out.println(inner1) // 输出 Outer$Inner@A
System.out.println(inner2) // 输出 Outer$Inner@B

System.out.println(inner1 == inner2) // 输出 false
```

同时，静态外部类无法访问外部类的非静态字段：

```java
class Outer {
    private final String id = "id1";

    // 此处有 static 修饰符
    static class Inner {
        // 无法运行
        System.out.println(Outer.this.id)
    }
}
```

### 局部内部类

方法内部的类，说实话感觉没啥用。

只能在方法内部使用，但局部内部类可以访问外部类的成员。

```java
void method() {
    // 代码...
    class Inner {
        // 内部代码...
    }

    // 直接创建就行
    Inner inner = new Inner();
}
```


### 匿名内部类

没名字的实例吧。

但是比局部内部类有用，好像后面的lambda就是匿名内部类的简化。

匿名内部类的创建方法，不过目前没法使用：

```java
class Sample{ 
    // 代码 ...
}

void main() {
    new Sample() {
        // 代码...
    }
}
```

需要一个方法去调用匿名内部类：

```java
class Sample{ 
    void method() {
        // 代码...
    }
}

void main() {
    call(new Sample() {
        void method() {
            // 代码...
        }
    });
}

void call(Sample s) {
    s.method();
}
```

## Lambda 表达式

lambda表达式和匿名内部类类似，但是lambda表达式只能用来简化**函数式接口（FunctionalInterface）**。

以下是用内部类实现的方法：

```java
// 函数式接口
@FunctionalInterface
interface Inter {
    void method();
}

void main() {
    call(
        // Inter 匿名内部类
        new Inter() {
            @Override
            public void method() {
                System.out.println("Hello");
            }
        }
    );
}

void call(Inter inter) {
    inter.method()
}
```

用lambda表达式就可以把中间的匿名内部类替换掉：

```java
void main() {
    call(
        // 接口的 method 是无参数方法，所以就只有一个括号。
        () -> {
            System.out.println("Hello");
        }
    );
}
```

当然，接口内的方法可以添加参数：

```java
// 函数式接口
@FunctionalInterface
interface Inter {
    void method(String str);
}

void main() {
    call(
        // 括号内填参数。
        (str) -> {
            System.out.println(str);
        }
    );
}

void call(Inter inter) {
    inter.method("Hello")
}
```

如上，参数都能一一对上，且表达式内部只有一个方法的调用话，就可以用**方法引用（MethodReference）**：

```java
void main() {
    call(System.out::println); // 输出 "Hello"
}

void call(Inter inter) {
    inter.method("Hello")
}
```