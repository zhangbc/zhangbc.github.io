---
title: 【Java基础】Java入门知识
type: categories
copyright: false
date: 2019-04-08 23:52:05
tags: Java基础
categories: Java
title_en: java_introductory_knowledge
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

## 1，java简介
   Java是由Sun Microsystems公司于1995年5月推出的Java面向对象程序设计语言和Java平台的总称。

   1）**Java分为三个体系**：
   > - JavaSE(J2SE)Java2 Platform Standard Edition，java平台标准版）
   > - JavaEE(J2EE)(Java 2 Platform,Enterprise Edition，java平台企业版)
   > - JavaME(J2ME)(Java 2 Platform Micro Edition，java平台微型版)
   
   2）**Java的主要特性**：
   > - java语言是简单的；
   > - java语言是面向对象的（纯面向对象）；
   > - java语言的分布式的；
   > - java语言是健壮的（丢弃指针，强类型机制，异常处理，垃圾的自动收集）；
   > - java语言是安全的（安全防范机制（类ClassLoader），安全管理机制（类SecurityManager））；
   > - java语言是可移植的；
   > - java语言是解释型的；
   > - java是高性能的；
   > - java语言多线程的；
   > - java语言是动态的。
   
   3）**Java的发展史**：诞生于1995年；2014年3月18日，Oracle公司发表Java SE8。
   
   4）**Java工具**：Java语言尽量保证系统内存在1G以上。

## 2，Java开发环境配置
1）下载[**JDK工具**](http://www.oracle.com/technetwork/java/javase/downloads/index.html)解压安装，对应不同的系统选择适合的版本。

2）**变量设置参数**如下：
> - 变量名：`JAVA_HOME`
> - 变量值：`C:\Program Files (x86)\Java\jdk1.8.0_91`        **// 要根据自己的实际路径配置**
> - 变量名：`CLASSPATH`
> - 变量值：`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`   **//记得前面有个"`.`"**
> - 变量名：`Path`
> - 变量值：`%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;`

3）**测试JDK是否安装成功**
```shell
~ java -version  # 输出java安装的版本
~ javac -version # 输出javac安装的版本
```
**`注意`**：如果使用1.5以上版本的JDK，不用设置CLASSPATH环境变量，也可以正常编译和运行Java程序。

4）**Java开发工具选择**
 -  [Eclipse](https://www.eclipse.org/downloads/)
 - [IntelliJ IDEA](https://www.jetbrains.com/idea/)(推荐)
 
## 3，Java基础语法
1）**相关概念**
   **`类`**：类是一个模板，描述一个对象的行为和状态。
    java中的`类`：  
   > - `局部变量`：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
   > - `成员变量`：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
   > - `类变量`：类变量也声明在类中，方法体之外，但必须声明为`static`类型。
   > - `构造方法`：每个类都有构造方法。如果没有显式地为类定义构造方法，Java编译器将会为该类提供一个默认构造方法。构造方法的名称必须与类同名，一个类可以有多个构造方法。 

   创建对象：在`Java`中，使用关键字`new`创建一个新的对象，主要有三步：`声明`，`实例化（new）`，`初始化`。
   `对象`：对象是一个类的实例，有状态和行为。
   在软件开发中，方法操作对象内部状态的改变，对象的相互调用也是通过方法来完成。
    `方法`：方法即行为，一个类可以有很多方法。
    `实例变量`：每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定。

2）**编程注意点**

> - 大小写敏感
> - 类名每个单词的首字母大写（帕斯卡命名法）
> - 方法名以小写字母开头，之后每个单词首字母大写（驼峰命名法）
> - 源文件名必须和类名相同
> - 主方法入口，所有的`Java`程序由```public static void main(String []args)```方法开始执行
        
3）**Java标识符**

`标识符`：类名，变量名及方法名都称为`标识符`。
> - 所有的`标识符`都应该以字母（`A-Z`或者`a-z`）,美元符（`$`）、或者下划线（`_`）开始；
> - 首字符之后可以是字母（`A-Z`或者`a-z`）,美元符（`$`）、下划线（`_`）或数字的任何字符组合；
> - `关键字`不能用作标识符；
> - `标识符`是大小写敏感的；
> - 合法标识符举例：`age`、`$salary`、`_value`、`__1_value`；
> - 非法标识符举例：`123abc`、`-salary`；

4）**Java修饰符**

（1）访问控制： `default`, `public`, `protected`, `private` 

| 修饰符   | 当前类 | 同一包内|子孙类（同一包）|子孙类（不同包）| 其他包 | 
|:-------:|:-----:|:------:|:-----------:|:-----------:|:------:|
|public      | Y          | Y            |Y                          | Y                          | Y         |
|protected  | Y          | Y            |Y                          | Y /N                     | N         |
|default 	  | Y          | Y            |Y                          | N                         | N         |
|private      | Y          | N            |N                          | N                         | N         |

**`protected`说明**：
> - 子类与基类在同一包中：被声明为`protected`的变量、方法和构造器能被同一个包中的任何其他类访问；
> - 子类与基类不在同一包中：那么在子类中，子类实例可以访问其从基类继承而来的 protected 方法，而不能访问基类实例的`protected`方法。

（2）访问控制和继承，注意以下原则：
> - 父类中声明为`public`的方法在子类中也必须为`public`。
> - 父类中声明为`protected`的方法在子类中要么声明为`protected`，要么声明为 `public`，不能声明为`private`。
> - 父类中声明为`private`的方法，不能够被继承。                

（3）非访问控制：`final`, `abstract`,`static`, `synchronized`,`volatile`
> - `static`：创建类方法和类变量；
> - `final`：修饰类，方法和变量。修饰的类不可被继承；修饰的方法不能被继承的类重新定义；修饰的变量为常量，不可修改。
> - `abstract`：创建抽象类和抽象方法；
> - `synchronized`：用于线程编程，synchronized声明的方法同一时间只能被一个线程访问；
> - `transient`：序列化的对象包含被 `transient`修饰的实例变量时，`Java`虚拟机(`JVM`)跳过该特定的变量；该修饰符包含在定义变量的语句中，用来预处理类和变量的数据类型。
> - `volatile`：用于线程编程， 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值，当成员变量发生变化时，会强制线程将变化值回写到共享内

5）**Java变量**

`局部变量`：类的方法中的变量。
> - 局部变量声明在方法、构造方法或者语句块中；
> - 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁；
> - 访问修饰符不能用于局部变量；
> - 局部变量只在声明它的方法、构造方法或者语句块中可见；
> - 局部变量是在`栈`上分配的。
> - 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。

`类变量（静态变量）`：独立于方法之外的变量，用`static`修饰。
`成员变量（非静态变量）`：独立于方法之外的变量，不用`static`修饰。

6）**Java数组**：储存在堆上的对象，可以保存多个同类型变量。

7）**Java枚举**：Java 5.0引入了枚举，枚举限制变量只能是预先设定好的值。使用枚举可以减少代码中的bug。

8）**Java关键字**：参见[Java关键字列表](http://www.runoob.com/java/java-basic-syntax.html)

9）**Java注释**

10）**Java空行**：空白行，或者有注释的行，Java编译器都会忽略掉。

11）**Java继承**
在Java中，一个类可以由其他类派生。被继承的类称为`超类`（`super class`），派生类称为`子类`（`subclass`）。

12）**Java接口**：在Java中，接口可以理解为对象间相互通信的协议。

13）**源文件声明规则**

> - 一个源文件中只能有一个public类
> - 一个源文件可以有多个非public类
> - 源文件的名称应该和public类的类名保持一致
> - 如果一个类定义在某个包中，那么package语句应该在源文件的首行
> - 如果源文件包含import语句，那么应该放在package语句和类定义之间；如果没有package语句，那么import语句应该在源文件中最前面。
> - import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。

14）**Java包**：包主要用来对类和接口进行分类。

15）**Import语句**

16）**Java运算符**

> - 算术运算符
> - 关系运算符
> - 位运算符：Java定义类位运算符，应用于整数类型（int），长整型（long），短整型（short），字符型（char）和字节型（byte）等类型。
> - 逻辑运算符
> - 赋值运算符
> - 其他运算符（instanceof，自增，自减，条件运算符）

17）**Java 源程序与编译型运行区别**
![Java 源程序与编译型运行区别](/images/java_intrductory_knowledge_20190408.png)
