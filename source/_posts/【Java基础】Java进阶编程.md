---
title: 【Java基础】Java进阶编程
type: categories
copyright: false
date: 2019-04-11 22:05:07
tags: Java基础
categories: Java
title_en: java_advance_program
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

## 1，Java数据结构

`Java`工具包提供了强大的数据结构，在`Java`中的数据结构主要包括以下接口和类：`枚举（Enumeration）`，`位集合（BitSet）`，`向量（Vector）`，`栈（Stack）`，`字典（Dictionary）`，`哈希表（Hashtable）`，`属性（Properties）`。
    
1）**枚举（`Enumeration`）**：该接口定义了一种从数据结构中取回连续元素的方式。

2）**位集合（`BitSet`）**：该类实现了一组可以单独设置和清除的位或标志。

3）**向量（`Vector`）**：对象的元素通过索引访问，在创建时不必给对象指定大小，其大小会根据需要动态的变化。

4）**栈（`Stack`）**：实现了一个后进先出的数据结构，是`Vector`的一个子类。

5）**字典（`Dictionary`）**：是一个抽象类，定义了键映射到值的数据结构。当想要通过特定的键而不是整数索引来访问数据时，应使用`Dictionary`。
**`注意`**：`Dictionary`类已经过时了，在实际开发中，你可以实现`Map`接口来获取键/值的存储功能。

6）**哈希表（`Hashtable`）**：提供了一种在用户定义键结构的基础上来组织数据的手段。`Hashtable`是原始的`java.util`的一部分， 是一个`Dictionary`具体的实现 。

7）**属性（`Properties`）**：继承于 Hashtable.Properties 类表示了一个持久的属性集；属性列表中每个键及其对应值都是一个字符串。

## 2，Java集合框架

1）**集合框架设计目标**
>（1）必须是高性能的，基本集合（动态数组，链表，树，哈希表）的实现也必须是高效的；
>（2）允许不同类型的集合，以类似的方式工作，具有高度的互操作性；
>（3）对一个集合的扩展和适应必须是简单的。

2）**Java集合框架图**

![Java集合框架图](/images/java_advance_program_collection_20190411.png)

3）**集合框架是一个用来代表和操纵集合的统一架构**，包含如下内容：
>（1）`接口`：是代表集合的抽象数据类型。例：`Collection`，`List`，`Set`，`Map`等。
>（2）`实现（类）`：是集合接口的具体实现。从本质上，他们是可重复使用的数据结构，例：`ArrayList`，`LinkedList`，`HashSet`，`HashMap`。
>（3）`算法`：是实现集合接口的对象里的方法执行的一些有用的计算，例：搜索和排序，这些算法被称为多态，因为相同的方法可以在相似的接口上有着不同的实现。

![Java 集合框架构成](/images/java_advance_program_collection_members_20190411.png)

集合框架的类和接口均在`java.util`包中。
任何对象加入集合类后，自动转变为`Object`类型，所以在取出的时候，需要进行强制类型转换。

4）**集合接口**
- `Set`和`List`的区别
> - `Set` 接口实例存储的是无序的，不重复的数据。`List` 接口实例存储的是有序的，可以重复的元素。
> - `Set` 检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变 <实现类有`HashSet`,`TreeSet`>。
> - `List`和数组类似，可以动态增长，根据实际存储的数据的长度自动增长`List`的长度。查找元素效率高，插入删除效率低，因为会引起其他元素位置改变 <实现类有`ArrayList`,`LinkedList`,`Vector`> 。
    
 5）**集合实现类（集合类）**：`Java`提供了一套实现了`Collection`接口的标准集合类。

 6）**集合算法**：集合定义了三个不可改变的静态变量：`EMPTY_SET`，`EMPTY_LIST`，`EMPTY_MAP`。`Collection Algorithms`是一个列表中的所有算法实现。
 
 7）**迭代**：使用`Java Iterator`，通过实例列出`Iterator`和`listIterator`接口提供的所有方法。

- ArrayListDemo.java
```java
public class ArrayListDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("Hello");
        list.add("World");
        list.add("Maven");
        list.add("Demo");
​
        // 遍历1：使用foreach遍历List
        System.out.println("使用foreach遍历List:");
        for (String str: list) {
            System.out.print(str + " ");
        }
​
        // 遍历2：把链表变为数组相关的内容进行遍历List
        String[] strArray = new String[list.size()];
        list.toArray(strArray);
        System.out.println("\n把链表变为数组相关的内容进行遍历List:");
        for (int i = 0; i < strArray.length; i++) {
            System.out.print(strArray[i] + " ");
        }
​
        // 遍历3：使用迭代器进行相关遍历List
        Iterator<String> iterator = list.iterator();
        System.out.println("\n使用迭代器进行相关遍历List:");
        while (iterator.hasNext()) {
            System.out.print(iterator.next() + " ");
        }
    }
}
```

- MapDemo.java
```java
public class MapDemo {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
​
        // 遍历1：通过Map.KeySet遍历
        System.out.println("通过Map.KeySet遍历key与value：");
        for (String key: map.keySet()) {
            System.out.println("key=" + key + ", value=" + map.get(key));
        }
​
        // 遍历2：通过Map.entrySet使用iterator遍历
        System.out.println("通过Map.entrySet使用iterator遍历key和value：");
        Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, String> entry = iterator.next();
            System.out.println("key=" + entry.getKey() + ", value=" + entry.getValue());
        }
​
        // 遍历3：推荐，通过Map.entrySet遍历
        System.out.println("通过Map.entrySet遍历key和value:");
        for (Map.Entry<String, String> entry:
             map.entrySet()) {
            System.out.println("key=" + entry.getKey() + ", value=" + entry.getValue());
        }
​
        // 遍历4：通过Map.values()遍历
        System.out.println("通过Map.values()遍历所有的value，但不能遍历key：");
        for (String val: map.values()) {
            System.out.println("value=" + val);
        }
    }
}
```

8）**比较器**：使用 `Java Comparator`，通过实例列出`Comparator`接口提供的所有方法。

## 3，Java泛型

1）**Java泛型（`generics`）** 是`JDK5`中引入的一个新特性，提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。`泛型的本质`是参数化类型，即：所操作的数据类型被指定为一个参数。

2）**定义泛型方法的规则**
> - 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前（在下面例子中的`<E>`）。
> - 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个`类型变量`，是用于指定一个泛型类型名称的标识符。
> - 类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符。
> - 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（如`int`,`double`,`char`的等）。 
 
```java
/**
 * 泛型方法实例
 */
class GenericMethod {
    public static void main(String[] args) {
        // 创建不同类型的数组：Integer，Double和Character
        Integer[] intArray = {1, 2, 3, 4, 5};
        Double[] doubleArray = {1.1, 2.2, 3.3, 4.4, 5.5};
        Character[] charArray = {'H', 'E', 'L', 'L', '0'};
​
        System.out.println("整型数组元素为：");
        printArray(intArray);
        System.out.println("双精度小数数组元素为：");
        printArray(doubleArray);
        System.out.println("字符型数组元素为：");
        printArray(charArray);
    }
​
    /**
     * 泛型方法printArray
     * @param inputArray
     * @param <E>
     */
    public static <E> void printArray(E[] inputArray) {
        for (E element: inputArray) {
            System.out.printf("%s ", element);
        }
        System.out.println();
    }
}
```

3）**有界的类型参数**：限制那些被允许传递到一个类型参数的类型种类范围。声明首先要列出类型参数的名称，后跟 `extends` 关键字，最后紧跟它的上界。

```java
/**
 * 泛型的有界类型参数实例
 */
class MaxGenericMethod {
    public static void main(String[] args) {
        System.out.printf("%d,%d和%d中最大的数为%d.\n\n",
                3, 4, 5, maximum(3, 4, 5));
​
        System.out.printf("%.2f,%.2f和%.2f中最大的数为%.2f.\n\n",
                6.6, 7.7, 8.8, maximum(6.6, 7.7, 8.8));
​
        System.out.printf("%s,%s和%s中最大的数为%s.\n\n",
                "pear", "apple", "orange",
                maximum("pear", "apple", "orange"));
    }
​
    public static <T extends Comparable<T>> T maximum(T x, T y, T z) {
        T max = x;
        if (y.compareTo(max) > 0) {
            max = y;
        }
        if (z.compareTo(max) > 0) {
            max = z;
        }
        return max;
    }
}
```

4）**泛型类**：在类名后面添加类型参数声明部分。泛型类的类型参数声明部分也包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个`类型变量`，是用于指定一个泛型类型名称的标识符。因为他们接受一个或多个参数，这些类被称为`参数化的类`或`参数化的类型`。

```java
/**
 * 泛型类实例
 * @param <T>
 */
class Box<T> {
    private T t;
    public void add(T t) {
        this.t = t;
    }
​
    public T get() {
        return t;
    }
​
    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<>();
        Box<String> stringBox = new Box<>();
        integerBox.add(10);
        stringBox.add("菜鸟教程");
​
        System.out.printf("整型值为：%d\n", integerBox.get());
        System.out.printf("字符串为：%s\n", stringBox.get());
    }
}
```

5）**类型通配符**：一般使用`?`代替具体的类型参数。
> （1）类型通配符上限通过形如`List`来定义，如此定义就是通配符泛型值接受`Number`及其下层子类类型；
> （2）类型通配符下限通过形如`List<? super Number>`来定义，表示类型只能接受`Number`及其三层父类类型。

```java
/**
 * 类型通配符实例
 */
class Wildcard {
    public static void main(String[] args) {
        List<String> name = new ArrayList<>();
        List<Integer> age = new ArrayList<>();
        List<Number> number = new ArrayList<>();
​
        name.add("icon");
        age.add(18);
        number.add(314);
​
        getData(name);
        getUpperNumber(age);
        getUpperNumber(number);
    }
​
    public static void getData(List<?> data) {
        System.out.printf("data: %s\n", data.get(0));
    }
​
    public static void getUpperNumber(List<? extends Number> data) {
        System.out.println("data: " + data.get(0));
    }
}
```

## 4，Java序列化

1）**Java序列化**：`Java`提供了一种对象序列化的机制，该机制中，一个对象可以表示为一个字节序列，该字节序列包括该对象的数据，有关对象的类型的信息和存储在对象中数据的类型。
>（1）序列化一个对象，并将它发送到输出流。
```java
public final void writeObject(Object x) throws IOException
```
>（2）从流中取出下一个对象，并将对象反序列化。
```java
public final void readObject(Object x) throws IOException, ClassNotFundException
```

2）**完整`demo`实例**：

- Employee.java
```java
package runoob;
​
import java.io.*;
​
/**
 * Employee类实现Serializable接口
 * @author 张伯成
 * @date 2019/3/7 12:30
 */
public class Employee implements Serializable {
    public String name;
    public String address;
    public transient int SSN;
    public int number;
​
    public void mailCheck() {
        System.out.println("Mailing a check to " + name + " " + address);
    }
}
```

- SerializeDemo.java 

```java 
/**
 * SerializeDemo类，序列化对象
 */
class SerializeDemo {
    public static void main(String[] args) {
        Employee employee = new Employee();
        employee.name = "Reyan Ali";
        employee.address = "Phonkka kuan, Ambehta Peer.";
        employee.SSN = 11122333;
        employee.number = 101;
​
        try {
            FileOutputStream fileOut = new
                    FileOutputStream("employee.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(employee);
            out.close();
            fileOut.close();
            System.out.println("Serialized data is saved in employee.ser.");
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
​
```

-  DeserializeDemo.java

```java
/**
 * DeserializeDemo类，反序列化对象
 */
class DeserializeDemo {
    public static void main(String[] args) {
        Employee employee;
        try {
            FileInputStream fileIn = new FileInputStream("employee.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            employee = (Employee) in.readObject();
            in.close();
            fileIn.close();
        } catch (IOException ex) {
            ex.printStackTrace();
            return;
        } catch (ClassNotFoundException ex) {
            System.out.println("Employee class not found.");
            ex.printStackTrace();
            return;
        }
        System.out.println("Deserialize Employee...");
        System.out.println("Name: " + employee.name);
        System.out.println("Address: " + employee.address);
        System.out.println("SSN: " + employee.SSN);
        System.out.println("Number: " + employee.number);
    }
}
```
