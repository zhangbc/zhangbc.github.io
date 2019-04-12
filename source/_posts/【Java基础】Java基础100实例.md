---
title: 【Java基础】Java基础100实例
type: categories
copyright: false
date: 2019-04-12 22:52:13
tags: Java基础
categories: Java
title_en: java_code_100
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

- *通过[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)的初步学习，现将其教程训练代码汇聚成篇。*

# 菜鸟教程-Java Coding学习笔记 #

> 1. Applet应用程序实例
> 2. 文档注释演示实例
> 3. 序列化和反序列化
> 4. Socket编程--服务端实例
> 5. Socket编程--客户端实例
> 6. Java进阶知识
>> - 遍历演示
>> - Map遍历实例
>> - 泛型方法实例 
>> - 泛型的有界类型参数实例
>> - 泛型类实例
>> - 类型通配符实例
> 7. 发邮件(纯文本，HTML文本，附件)
> 8. 图片二进制转换
> 9. JAVA8新特性实例
> 10. JAVA操作MYSQL实例

# 菜鸟教程-Java实例 #

## Java 环境设置实例 ##
>> 1. Java实例 – 如何编译一个Java文件？
>> 2. Java实例 – Java如何运行一个编译过的类文件？
>> 3. Java实例 – 如何执行指定class文件目录（classpath）？
>> 4. Java实例 – 如何查看当前Java运行的版本？

```shell
$ javac -d . HelloWorld.java
$ java com.runoob.HelloWorld
```

## Java 字符串 ##
>> 1. Java 实例 – 字符串比较
>> 2. Java 实例 - 查找字符串最后一次出现的位置
>> 3. Java 实例 - 删除字符串中的一个字符
>> 4. Java 实例 - 字符串替换
>> 5. Java 实例 - 字符串反转
>> 6. Java 实例 - 字符串查找
>> 7. Java 实例 - 字符串分割
>> 8. Java 实例 - 字符串分割(StringTokenizer)
>> 9. Java 实例 - 字符串大小写转换
>> 10. Java 实例 - 测试两个字符串区域是否相等
>> 11. Java 实例 - 字符串性能比较测试
>> 12. Java 实例 - 字符串优化
>> 13. Java 实例 - 字符串格式化
>> 14. Java 实例 - 连接字符串

## Java 数组 ##
>> 1. Java 实例 – 数组排序及元素查找
>> 2. Java 实例 – 数组添加元素
>> 3. Java 实例 – 获取数组长度
>> 4. Java 实例 – 数组反转
>> 5. Java 实例 – 数组输出
>> 6. Java 实例 – 数组获取最大和最小值
>> 7. Java 实例 – 数组合并
>> 8. Java 实例 – 数组填充
>> 9. Java 实例 – 数组扩容
>> 10. Java 实例 – 查找数组中的重复元素
>> 11. Java 实例 – 删除数组元素
>> 12. Java 实例 – 数组差集
>> 13. Java 实例 – 数组交集
>> 14. Java 实例 – 在数组中查找指定元素
>> 15. Java 实例 – 判断数组是否相等
>> 16. Java 实例 - 数组并集

## Java 时间处理 ##
>> 1. Java 实例 - 格式化时间（SimpleDateFormat）
>> 2. Java 实例 - 获取当前时间
>> 3. Java 实例 - 获取年份、月份等
>> 4. Java 实例 - 时间戳转换成时间

## Java 方法 ##
>> 1. Java 实例 – 方法重载
>> 2. Java 实例 – 输出数组元素
>> 3. Java 实例 – 汉诺塔算法
>> 4. Java 实例 – 斐波那契数列
>> 5. Java 实例 – 阶乘
>> 6. Java 实例 – 方法覆盖
>> 7. Java 实例 – instanceOf 关键字用法
>> 8. Java 实例 – break 关键字用法
>> 9. Java 实例 – continue 关键字用法
>> 10. Java 实例 – 标签(Label)
>> 11. Java 实例 – enum 和 switch 语句使用
>> 12. Java 实例 – Enum（枚举）构造函数及方法的使用
>> 13. Java 实例 – for 和 foreach循环使用
>> 14. Java 实例 – Varargs 可变参数使用
>> 15. Java 实例 – 重载(overloading)方法中使用 Varargs

## 打印图形 ##
>> 1. Java 实例 – 打印菱形
>> 2. Java 实例 – 九九乘法表
>> 3. Java 实例 – 打印三角形
>> 4. Java 实例 – 打印倒立的三角形
>> 5. Java 实例 – 打印平行四边形
>> 6. Java 实例 – 打印矩形

## Java 文件操作 ##
>> 1. Java 实例 - 文件写入
>> 2. Java 实例 - 读取文件内容
>> 3. Java 实例 - 删除文件
>> 4. Java 实例 - 将文件内容复制到另一个文件
>> 5. Java 实例 - 向文件中追加数据
>> 6. Java 实例 - 创建临时文件
>> 7. Java 实例 - 修改文件最后的修改日期
>> 8. Java 实例 - 获取文件大小
>> 9. Java 实例 - 文件重命名
>> 10. Java 实例 - 设置文件只读
>> 11. Java 实例 - 检测文件是否存在
>> 12. Java 实例 - 在指定目录中创建文件
>> 13. Java 实例 - 获取文件修改时间
>> 14. Java 实例 - 创建文件
>> 15. Java 实例 - 文件路径比较

## Java 目录操作 ##
>> 1. Java 实例 - 递归创建目录
>> 2. Java 实例 - 删除目录
>> 3. Java 实例 - 判断目录是否为空
>> 4. Java 实例 - 判断文件是否隐藏
>> 5. Java 实例 - 获取目录大小
>> 6. Java 实例 - 在指定目录中查找文件
>> 7. Java 实例 - 获取文件的上级目录
>> 8. Java 实例 - 获取目录最后修改时间
>> 9. Java 实例 - 打印目录结构
>> 10. Java 实例 - 遍历指定目录下的所有目录
>> 11. Java 实例 - 遍历指定目录下的所有文件
>> 12. Java 实例 - 在指定目录中查找文件
>> 13. Java 实例 - 遍历系统根目录
>> 14. Java 实例 - 查看当前工作目录
>> 15. Java 实例 - 遍历目录

## Java 异常处理 ##
>> 1. Java 实例 - 异常处理方法
>> 2. Java 实例 - 多个异常处理（多个catch）
>> 3. Java 实例 - Finally的用法
>> 4. Java 实例 - 使用 catch 处理异常
>> 5. Java 实例 - 多线程异常处理
>> 6. Java 实例 - 获取异常的堆栈信息
>> 7. Java 实例 - 重载方法异常处理
>> 8. Java 实例 - 链试异常
>> 9. Java 实例 - 自定义异常

## Java 数据结构 ##
>> 1. Java 实例 – 数字求和运算
>> 2. Java 实例 – 利用堆栈将中缀表达式转换成后缀
>> 3. Java 实例 – 在链表（LinkedList）的开头和结
>> 4. Java 实例 – 获取链表（LinkedList）的第一个
>> 5. Java 实例 – 删除链表中的元素
>> 6. Java 实例 – 获取链表的元素
>> 7. Java 实例 – 获取向量元素的索引值
>> 8. Java 实例 – 栈的实现
>> 9. Java 实例 – 链表元素查找
>> 10. Java 实例 – 压栈出栈的方法实现字符串反转
>> 11. Java 实例 – 队列（Queue）用法
>> 12. Java 实例 – 获取向量的最大元素
>> 13. Java 实例 – 链表修改
>> 14. Java 实例 – 旋转向量

## Java 集合 ##
>> 1. Java 实例 – 数组转集合
>> 2. Java 实例 – 集合比较
>> 3. Java 实例 – HashMap遍历
>> 4. Java 实例 – 集合长度
>> 5. Java 实例 – 集合打乱顺序
>> 6. Java 实例 – 集合遍历
>> 7. Java 实例 – 集合反转
>> 8. Java 实例 – 删除集合中指定元素
>> 9. Java 实例 – 只读集合
>> 10. Java 实例 – 集合输出
>> 11. Java 实例 – 集合转数组
>> 12. Java 实例 – List 循环移动元素
>> 13. Java 实例 – 查找 List 中的最大最小值
>> 14. Java 实例 – 遍历 HashTable 的键值
>> 15. Java 实例 – 使用 Enumeration 遍历 HashTable
>> 16. Java 实例 – 集合中添加不同类型元素
>> 17. Java 实例 – List 元素替换
>> 18. Java 实例 – List 截取

## Java 网络实例 ##
>> 1. Java 实例 – 获取指定主机的IP地址
>> 2. Java 实例 – 查看端口是否已使用
>> 3. Java 实例 – 获取本机ip地址及主机名
>> 4. Java 实例 – 获取远程文件大小
>> 5. Java 实例 – Socket 实现多线程服务器程序
>> 6. Java 实例 – 查看主机指定文件的最后修改时间
>> 7. Java 实例 – 使用 Socket 连接到指定主机
>> 8. Java 实例 – 网页抓取
>> 9. Java 实例 – 获取 URL响应头的日期信息
>> 10. Java 实例 – 获取 URL 响应头信息
>> 11. Java 实例 – 解析 URL
>> 12. Java 实例 – ServerSocket 和 Socket 通信实例

## Java 线程 ##
>> 1. Java 实例 – 查看线程是否存活
>> 2. Java 实例 – 获取当前线程名称
>> 3. Java 实例 – 状态监测
>> 4. Java 实例 – 线程优先级设置
>> 5. Java 实例 – 死锁及解决方法
>> 6. Java 实例 – 获取线程id
>> 7. Java 实例 – 线程挂起
>> 8. Java 实例 – 终止线程
>> 9. Java 实例 – 生产者/消费者问题
>> 10. Java 实例 – 获取线程状态
>> 11. Java 实例 – 获取所有线程
>> 12. Java 实例 – 查看线程优先级
>> 13. Java 实例 – 中断线程

- [**github**](https://github.com)代码送门: [java_projects/runoob](https://github.com/zhangbc/java_projects/tree/runoob)  
-  [**腾讯云coding**](https://dev.tencent.com/user)代码送门: [java_project/dev](https://git.dev.tencent.com/zhangbocheng/java_project.git/tree/dev)