---
title: 【Java基础】Java扩展知识
type: categories
copyright: false
date: 2019-04-12 22:19:34
tags: Java基础
categories: Java
title_en: java_extend_knowledge
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

## 1，Java文档注释

1）**`Java`支持三种注释方式**，分别是`//`、`/*  */`、`/**  */`(说明注释)。

2）**`javadoc`标签**

| 标签                | 描述|	示例|
|:-------------------|:-------|:--------|
|@author          | 标识一个类的作者                 | @author description |
|@deprecated  | 指名一个过期的类或成员       |	@deprecated description |
|{@docRoot}    | 指明当前文档根目录的路径    | Directory Path |
|@exception    | 标志一个类抛出的异常	          | @exception exception-name explanation |
|{@inheritDoc} | 从直接父类继承的注释            | Inherits a comment from the immediate surperclass. |
|{@link}            | 插入一个到另一个主题的链接 | {@link name text} |
|{@linkplain}    | 插入一个到另一个主题的链接，但是该链接显示纯文本字体 | Inserts an in-line link to another topic. |
|@param	        | 说明一个方法的参数                | @param parameter-name explanation |	
|@return          | 说明返回值类型                       | @return explanation |	
|@see	            | 指定一个到另一个主题的链接 |	@see anchor |	
|@serial           | 说明一个序列化属性	              |	@serial description |	
|@serialData   | 明通过writeObject( ) 和 writeExternal( )方法写的数据 |	@serialData description |
|@serialField   | 说明一个ObjectStreamField组件 |	@serialField name type description |
|@since           | 标记当引入一个特定的变化时  | @since release |
|@throws	        | 和 @exception标签一样.	       | The @throws tag has the same meaning as the @exception tag. |
|{@value}         | 显示常量的值，该常量必须是static属性。| Displays the value of a constant, which must be a static field. |
|@version	    | 指定类的版本	                       | @version info |

- 示例：
```java
package com.runoob;
​
​
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
​
/**
 * 文档注释演示实例
 * @author zhangbc
 * @version 1.0
 */
public class SquareNumber {
    /**
     * This method returns the square of number.
     * This is a multiline description. You can use as many lines as you like.
     * @param number The value to be squared.
     * @return number squared.
     */
    public double square(double number) {
        return number * number;
    }
​
    /**
     * This method input a number from the user.
     * @return The value input as a double.
     * @throws IOException in input error
     * @see IOException
     */
    public double getNumber() throws IOException {
        InputStreamReader isr = new InputStreamReader(System.in);
        BufferedReader inData = new BufferedReader(isr);
        String str;
        str = inData.readLine();
        return Double.parseDouble(str);
    }
​
    /**
     * This method demonstrates square().
     * @param args args unused.
     * @throws IOException on input error.
     * @see IOException
     */
    public static void main(String[] args) throws IOException {
        SquareNumber sn = new SquareNumber();
        double val;
        System.out.print("Enter value to be squared:");
        val = sn.getNumber();
        val = sn.square(val);
        System.out.println("Squared value is : " + val);
    }
}
```

## 2，Java 8 新特性

1）**`Java8`(即`jdk1.8`)新特性**

>（1）`Lambda` 表达式：`Lambda`允许把函数作为一个方法的参数（函数作为参数传递进方法中。
>（2）方法引用：可以直接引用已有`Java`类或对象（实例）的方法或构造器。与`lambda`联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
>（3）默认方法：默认方法就是一个在接口里面有了一个实现的方法。
>（4）新工具：新的编译工具，如：`Nashorn`引擎` jjs`、 类依赖分析器`jdeps`。
>（5）`Stream API `：把真正的函数式编程风格引入到`Java`中。
>（6）`Date Time API`：加强对日期与时间的处理。
>（7）`Optional` 类：`Optional` 类已经成为 `Java 8` 类库的一部分，用来解决空指针异常。
>（8）`Nashorn`, `JavaScript` 引擎：允许我们在`JVM`上运行特定的`javascript`应用。

## 3，Java MySQL连接

- MysqlDemo.java
```java
package com.runoob;
​
import java.sql.*;
​
/**
 * 连接数据库实例
 * @author zhangbc
 * @version v1.0
 * @date 2019/3/28 22:14
 */
public class MysqlDemo {
​
    /**
     * JDBC驱动名及其数据库URL
     */
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://127.0.0.1:3306/pyspider_db";
​
    /**
     * 数据库的用户与密码
     */
    static final String USER = "root";
    static final String PASSWORD = "xxxxxx";
​
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            Class.forName(JDBC_DRIVER);
            System.out.println("连接数据库...");
            conn = DriverManager.getConnection(DB_URL, USER, PASSWORD);
​
            System.out.println("实例化Statement对象...");
            stmt = conn.createStatement();
            String sql;
            sql = "select id, name, url from websites;";
            ResultSet rs = stmt.executeQuery(sql);
​
            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                String url = rs.getString("url");
​
                System.out.printf("ID: %d\t站点名称：%s\t站点URL：%s\n", id, name, url);
            }
​
            rs.close();
            stmt.close();
            conn.close();
        } catch (SQLException se) {
            se.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (stmt != null) {
                    stmt.close();
                }
            } catch (SQLException se) {
                se.printStackTrace();
            }
​
            try {
                if (conn != null) {
                    conn.close();
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 4，Java 9 新特性
详情参见：[Java 9 新特性](http://www.runoob.com/java/java9-new-features.html)
