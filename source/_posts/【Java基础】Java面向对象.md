---
title: 【Java基础】Java面向对象
type: categories
copyright: false
date: 2019-04-11 19:57:07
tags: Java基础
categories: Java
title_en: java_object_oriented
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

## 1，Java 继承
1）**`Java`继承的概念**：`继承`就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。

2）**`Java`继承类型**：Java不支持多继承，但是支持多重继承。
![Java继承](/images/java_obj_oriented_extends_20190411.png)
3）**`Java`继承的特性**

> - 子类拥有父类非 `private` 的属性、方法。
> - 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
> - 子类可以用自己的方式实现父类的方法。
> - `Java` 的继承是`单继承`，但是可以`多重继承`，单继承就是一个子类只能继承一个父类，多重继承就是，例如 A 类继承 B 类，B 类继承 C 类，所以按照关系就是 C 类是 B 类的父类，B 类是 A 类的父类，这是 Java 继承区别于 C++ 继承的一个特性。
> - 提高了类之间的`耦合性`（继承的缺点，耦合度高就会造成代码之间的联系越紧密，代码独立性越差）。

4）**`Java`继承关键字**

`继承`可以使用`extends`和`implements`这两个关键字来实现继承，都继承于`java.lang.Object`，默认继承`object`祖先类。
>（1）`extends`：只能继承一个类。
>（2）`implements`：可以变相地使`java`具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口。
>（3）`super`：通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。
>（4）`this`：指向自己的引用。
>（5）`finally`：声明类可以把类定义为不能继承的，即`最终类`；或用于修饰方法，该方法不能被子类重写。
  
 5）**`Java`构造器**
 子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 `super` 关键字调用父类的构造器并配以适当的参数列表。
 
 - ExtendsDemo.java
```java
package com.example.springboot;
​
/**
 * @function:
 * @author: 张伯成
 * @date: 2019/3/3
 */
public class ExtendsDemo {
    public static void main(String[] args) {
        System.out.println("==========SubClassOne 类继承===========");
        SubClassOne sc1 = new SubClassOne();
        SubClassOne sc2 = new SubClassOne(100);
        System.out.println("==========SubClassTwo 类继承===========");
        SubClassTwo sc3 = new SubClassTwo();
        SubClassTwo sc4 = new SubClassTwo(200);
    }
}
​
​
/**
 * SuperClass 祖先类
 */
class SuperClass {
    private int number;
    SuperClass() {
        System.out.println("SuperClass()");
    }
​
    SuperClass(int number) {
        this.number = number;
        System.out.println("SuperClass(int number)");
    }
}
​
​
/**
 * SubClassOne 类继承
 */
class SubClassOne extends SuperClass {
    private int number;
    SubClassOne() {
        System.out.println("SubClassOne()");
    }
​
    SubClassOne(int number) {
        super(300);
        this.number = number;
        System.out.println("SubClassOne(int number): " + number);
    }
}
​
​
/**
 * SubClassTwo 类继承
 */
class SubClassTwo extends SuperClass {
    private int number;
    SubClassTwo() {
        super(300);
        System.out.println("SubClassTwo()");
    }
​
    SubClassTwo(int number) {
        this.number = number;
        System.out.println("SubClassTwo(int number): " + number);
    }
}
```

## 2，Java Override/Overload

1）**重写（`Override`）**
`重写`是子类对父类对允许访问的方法的实现过程进行重新编写，返回值和形参都不能改变，即`外壳不变，核心重写`。重写方法不能抛出新的检查异常或者比被重写方法声明更加宽泛的异常。
```java
/**
 * Animal类，祖先类
 */
class Animal {
    public void move() {
        System.out.println("动物可以移动...");
    }
}
​
​
/**
 * AnimalDog类，继承Animal，并重写父类的move()方法
 */
class AnimalDog extends Animal {
    public void move() {
        super.move();
        System.out.println("狗可以跑和走.");
    }
}
​
​
class DogTest {
    public static void main(String[] args) {
        Animal animal = new Animal();
        Animal dog = new AnimalDog();
        animal.move();
        dog.move();
    }
}
```

2）**方法的重写规则**

> - 参数列表必须完全与被重写方法的相同。
> - 返回类型必须完全与被重写方法的返回类型相同。
> - 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为`public`，那么在子类中重写该方法就不能声明为`protected`。
> - 父类的成员方法只能被它的子类重写。
> - 声明为`final`的方法不能被重写。
> - 声明为`static`的方法不能被重写，但是能够被再次声明。
> - 子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为`private`和`final`的方法。
> - 子类和父类不在同一个包中，那么子类只能够重写父类的声明为`public`和`protected`的非`final`方法。
> - 重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
> - 构造方法不能被重写。
> - 如果不能继承一个方法，则不能重写这个方法。

3）**`super`**：当需要在子类中调用父类的被重写方法时，要使用`super`关键字。

4）**重载（`Overload`）**：在一个类里面，方法名字相同，而参数不同，返回类型可同可不同。

5）**重载规则**

> - 被重载的方法必须改变参数列表(参数个数或类型不一样)；
> - 被重载的方法可以改变返回类型；
> - 被重载的方法可以改变访问修饰符；
> - 被重载的方法可以声明新的或更广的检查异常；
> - 方法能够在同一个类中或者在一个子类中被重载；
> - 无法以返回值类型作为重载函数的区分标准。

- OverLoading.java
```java
public class OverLoading {
    public int test() {
        System.out.println("test()");
        return 1;
    }
​
    public void test(int number) {
        System.out.println("test(int)");
    }
​
    public String test(int number, String str) {
        System.out.println("test(int, String)");
        return "test(int, String)";
    }
​
    public String test(String str, int number) {
        System.out.println("test(String,int)");
        return "test(String,int)";
    }
​
    public static void main(String[] args) {
        OverLoading overLoad = new OverLoading();
        System.out.println(overLoad.test());
        overLoad.test(100);
        System.out.println(overLoad.test(100, "test3"));
        System.out.println(overLoad.test("test", 100));
    }
}
```

6）**`重写`和`重载`的区别**

| 区别点     | 重载方法 |  重写方法                                                                | 
|:-------------|:-------------|:------------------------------------------------------------------|
| 参数列表 | 必须修改 | 一定不能修改                                                          |
| 返回类型 | 可以修改 | 一定不能修改                                                          |
| 异常        | 可以修改 | 可以减少或删除，一定不能抛出新的或更广的异常 |
| 访问        | 可以修改 | 一定不能做更严格的限制（可以降低限制）            |

方法的重写(`Overriding`)和重载(`Overloading`)是`java`多态性的不同表现，`重写`是父类与子类之间多态性的一种表现，`重载`可以理解成多态的具体表现形式。
> - 方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为`方法的重载(Overloading)`。
> - 方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为`重写(Overriding)`。
> - 方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。
        
## 3，Java 多态
`多态`是同一个行为具有多个不同表现形式或形态的能力。**多态的好处**：可以使程序有良好的扩展，并可以对所有类的对象进行通用处理。

1）**多态的优点**：消除类型之间的耦合关系；可替换性；可扩充性；接口性；灵活性；简化性。

2）**多态存在的三个必要条件**：`继承`；`重写`；`父类引用指向子类对象`。

- PolymorphicDemo.java
```java
package com.example.springboot;
​
/**
 * @function:
 * @author: 张伯成
 * @date: 2019/3/3
 */
public class PolymorphicDemo {
    public static void main(String[] args) {
        show(new Cat());
        show(new Dog());
​
        Animals animal = new Cat();
        animal.eat();
​
        Cat cat = (Cat) animal;
        cat.work();
    }
​
    public static void show(Animals animal) {
        animal.eat();
​
        if (animal instanceof Cat) {
            Cat cat = (Cat) animal;
            cat.work();
        } else if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.work();
        }
    }
}
​
​
abstract class Animals {
    abstract void eat();
}
​
​
class Cat extends Animals {
    public void eat() {
        System.out.println("吃鱼");
    }
​
    public void work() {
        System.out.println("抓老鼠");
    }
}


class Dog extends Animals {
    public void eat() {
        System.out.println("吃骨头");
    }
​
    public void work() {
        System.out.println("看家");
    }
}
```

3）**虚函数**：虚函数的存在是为了多态。Java 中其实没有虚函数的概念，它的普通函数就相当于 C++ 的虚函数，动态绑定是Java的默认行为。

4）**多态的实现方式**：`重写`；`接口`；`抽象类`和`抽象方法`。

## 4，Java 抽象类
在Java中抽象类表示的是一种继承关系，一个类只能继承一个抽象类，而一个类却可以实现多个接口。

1）**抽象类**：在Java语言中使用`abstract class`定义抽象类。

2）**继承抽象类**

3）**抽象方法**：`abstract` 关键字可以用来声明抽象方法，抽象方法只包含一个方法名，而没有方法体；抽象方法没有定义，方法名后面直接跟一个分号，而不是花括号。

4）**抽象类总结规定**
>（1）抽象类不能被实例化；只有抽象类的非抽象子类可以创建对象；
>（2）抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类；
>（3）抽象类中的抽象方法只是声明，不包含方法体，就是不给出方法的具体实现也就是方法的具体功能；
>（4）构造方法，类方法（用 `static` 修饰的方法）不能声明为抽象方法；
>（5）抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类。

## 5，Java 封装

1）在面向对象程式设计方法中，`封装（Encapsulation）`是指一种将抽象性函式接口的实现细节部分包装、隐藏起来的方法。

封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。

要访问该类的代码和数据，必须通过严格的`接口`控制。

`封装` **最主要的功能**在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段。

2）**封装的优点**
>（1）两个的封装能够减少耦合；
>（2）类内部的结构可以自由修改；
>（3）可以对成员变量进行更精确的控制；
>（4）隐藏信息，实现细节。

## 6，Java 接口

1）**接口概念**
     `接口（Interface）`，在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
     
 类描述对象的属性和方法。接口则包含类要实现的方法。
   
 除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。

 接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在 `Java` 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。

2）**接口与类相似点**
> - 一个接口可以有多个方法。
> - 接口文件保存在` .java` 结尾的文件中，文件名使用接口名。
> - 接口的字节码文件保存在 `.class` 结尾的文件中。
> - 接口相应的字节码文件必须在与包名称相匹配的目录结构中。
    
3）**接口与类的区别**
> - 接口不能用于实例化对象。
> - 接口没有构造方法。
> - 接口中所有的方法必须是抽象方法。
> - 接口不能包含成员变量，除了 `static` 和 `final` 变量。
> - 接口不是被类继承了，而是要被类实现。
> - 接口支持多继承。

4）**接口特性**
> - 接口是隐式的，接口中每一个方法也是隐式抽象的，接口中的方法会被隐式的指定为 `public abstract`。
> - 接口中可以含有变量，但是接口中的变量会被隐式的指定为 `public static final` 变量。
> - 接口中的方法是不能在接口中实现的，只能由实现接口的类来实现接口中的方法。
> - 接口的方法都是公有的。

5）**抽象类和接口的区别**
> - 抽象类中的方法可以有方法体，就是能实现方法的具体功能，但是接口中的方法不行。
> - 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 `public static final` 类型的。
> - 接口中不能含有静态代码块以及静态方法(用 `static` 修饰的方法)，而抽象类是可以有静态代码块和静态方法。
> - 一个类只能继承一个抽象类，而一个类却可以实现多个接口。

6）**接口的声明**
```java
[可见度] interface 接口名称 [extends 其他的接口名名] {
        // 声明变量
        // 抽象方法
}
```

7）**接口的实现**：当类实现接口的时候，类要实现接口中所有的方法，否则，类必须声明为抽象的类；类使用`implements`关键字实现接口，在类声明中，`implements`关键字放在`class`声明后面。

（1）重写接口中声明的方法时，需要注意以下规则：

> - 类在实现接口的方法时，不能抛出强制性异常，只能在接口中，或者继承接口的抽象类中抛出该强制性异常。
> - 类在重写方法时要保持一致的方法名，并且应该保持相同或者相兼容的返回值类型。
> - 如果实现接口的类是抽象类，那么就没必要实现该接口的方法。

（2）在实现接口的时候，需要注意以下规则：

> - 一个类可以同时实现多个接口。
> - 一个类只能继承一个类，但是能实现多个接口。
> - 一个接口能继承另一个接口，这和类之间的继承比较相似。

8）**接口的继承**

9）**接口的多继承**：在`Java`中，类的多继承是不合法，但接口允许多继承；在接口的多继承中`extenfs`关键字只需要用一次，在其后跟着继承接口。

10）**标记接口**
    `标记接口`是没有任何方法和属性的接口，它仅仅表明它的类属于一个特定的类型，供其他代码来测试允许做一些事情。

`标记接口`的**作用**：简单形象的说就是给某个对象打个标（盖个戳），使对象拥有某个或某些特权。

`标记接口`的**主要目的**：建立一个公共的父接口；向一个类添加数据类型。

## 7，Java 包（Package）
 1）**包的作用**
  >（1）把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用。
  >（2）如同文件夹一样，包也采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。因此，包可以避免名字冲突。
   >（3）包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类。
     
 总之，`Java` 使用包（`package`）这种机制是为了防止命名冲突，访问控制，提供搜索和定位类（`class`）、接口、枚举（`enumerations`）和注释（`annotation`）等。
 
 2）**创建包**
 
 3）**`impor`t语句**：为了能够使用某一个包的成员，需要在`Java`程序中明确导入该包。
 
 4）**`package`的目录结构**
 类目录的绝对路径叫做 `class path`。设置在系统变量 `CLASSPATH` 中。编译器和 `java` 虚拟机通过将 `package` 名字加到 `class path` 后来构造 `.class` 文件的路径。

5）**设置`CLASSPATH`系统变量**
- 用下面的命令显示当前的`CLASSPATH`变量：
```shell
# Windows 平台（DOS 命令行下）
$ C:\> set CLASSPATH

# UNIX 平台（Bourne shell 下）
$ echo $CLASSPATH
```
- 删除当前`CLASSPATH`变量内容：
```shell
# Windows 平台（DOS 命令行下）
$ C:\> set CLASSPATH=
# UNIX 平台（Bourne shell 下）
$ unset CLASSPATH; export CLASSPATH
```
- 设置`CLASSPATH`变量：
```shell
# Windows 平台（DOS 命令行下）
$ C:\> set CLASSPATH=C:\users\jack\java\classes
# UNIX 平台（Bourne shell 下）
$ CLASSPATH=/home/jack/java/classes; export CLASSPATH
```