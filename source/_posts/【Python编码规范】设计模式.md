---
title: 【Python编码规范】设计模式
type: categories
copyright: false
date: 2019-06-13 22:55:38
tags: Python编码规范
categories: Python
title_en: python_code91_05
---


> 本系列为《编写高质量代码-改善Python程序的91个建议》的读书笔记。

**温馨提醒**：在阅读本书之前，强烈建议先仔细阅读：[**PEP**规范](https://legacy.python.org/dev/peps/pep-0008/)，增强代码的可阅读性，配合优雅的[pycharm](http://www.jetbrains.com/pycharm/)编辑器(开启`pep8`检查)写出规范代码，是`Python`入门的第一步。

## 建议50：利用模块实现单例模式

1）所有的变量都会绑定到模块；
2）模块只初始化一次；
3）`import`机制是线程安全的。

## 建议51: 用mixin模式让程序更加灵活

`模板方法模式`：在一个方法中定义一个算法的骨架，并将一些实现步骤延迟到子类中。

`Python` 中每一个类都有一个`__base__`属性，是一个元组，用来存放所有的基类，基类在运行中可以动态改变。

## 建议52：用发布订阅模式实现松耦合
    
 发布订阅模式（publish/subscribe或pub/sub）是一种编程模式，消息的发送者（发布者）不会发送其消息给特定的接收者（订阅者），而是将发布的消息分为不同的类别直接发布，并不关注订阅者是谁。

```python
#!/usr/bin/env python
# coding:utf-8
​
​
"""
发布订阅模式实现
"""
​
​
import message
from collections import defaultdict
​
​
route_table = defaultdict(list)
​
​
def sub(topic, callback):
    """
​
    :param topic:
    :param callback:
    :return:
    """
​
    if callback in route_table[topic]:
        return
​
    route_table[topic].append(callback)
​
​
def pub(topic, *args, **kwargs):
    """
​
    :param topic:
    :param args:
    :param kwargs:
    :return:
    """
​
    for func in route_table[topic]:
        func(*args, **kwargs)
​
​
def greeting(name):
    """
​
    :param name:
    :return:
    """
​
    print 'Hello, {0}.'.format(name)
​
​
if __name__ == '__main__':
​
    sub('greet', greeting)
    pub('greet', 'LaiYonghao')
​
    message.sub('greet', greeting)
    message.pub('greet', 'Welcome to Python')
```
   
## 建议53：用状态模式美化代码

`状态模式`：当一个对象的内在状态改变时允许改变其行为，但这个对象看起来像是改变了其类。主要用于控制一个对象状态的条件表达式过于复杂的情况，其可把状态的判断逻辑转移到表示不同状态的一系列类中，进而把复杂的判断逻辑简化。
@stateful修饰函数，重载了被修饰类的__getattr__()方法从而使得类的实例方法能调用当前状态类的方法。被@stateful修饰后的类的实例是带有状态的，能够使用curr()查询当前状态，也可以使用switch()进行状态切换。

```python
#!/usr/bin/env python
# coding:utf-8
​
​
"""
状态模式实现
"""
​
​
from state import switch, stateful, State, behavior
​
​
@stateful
class People(object):
    """
​
    """
​
    class Workday(State):
        """
​
        """
​
        default = True
​
        @behavior
        def day(self):
            print 'work hard.'
​
    class Weekend(State):
        """
​
        """
​
        @behavior
        def day(self):
            print 'play harder.'
​
​
def main():
    """
​
    :return:
    """
​
    people = People()
    for i in xrange(1, 8):
        if i == 6:
            switch(people, People.Weekend)
        if i == 1:
            switch(people, People.Workday)
        people.day()
​
​
if __name__ == '__main__':
​
    main()
```
