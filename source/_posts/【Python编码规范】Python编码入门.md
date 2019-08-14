---
title: 【Python编码规范】Python编码入门
type: categories
copyright: false
date: 2019-04-25 22:56:39
tags: Python编码规范
categories: Python
title_en: python_code91_01
---


> 本系列为《编写高质量代码-改善Python程序的91个建议》的读书笔记。

**温馨提醒**：在阅读本书之前，强烈建议先仔细阅读：[**PEP**规范](https://legacy.python.org/dev/peps/pep-0008/)，增强代码的可阅读性，配合优雅的[pycharm](http://www.jetbrains.com/pycharm/)编辑器(开启`pep8`检查)写出规范代码，是`Python`入门的第一步。

> *本书主要内容*
>> 1）容易被忽视的重要概念和常识，如代码的布局和编写函数的原则等；
>> 2）编写`Python`程序管用的方法，如利用`assert`语句去发现问题，使用`enumerate()`获取序列迭代的索引和值等；
>> 3）语法中的关键条款，如有节制地使用`from…import`语句，异常处理的几点基本原则等；
>> 4）常见库的使用，如按需选择`sort()`或者`sorted()`，使用`Queue`使多线程更安全等；
>> 5）`Python`设计模式的使用，如用发布订阅模式实现松耦合，用状态模式美化代码等；
>> 6）`Python`内部机制，如名字查找机制，描述符机制等；
>> 7）开发工具的使用，如`pip`等各种开发工具的使用，各种代码测试用具的使用等；
>> 8）`Python`代码的性能分析，优化的原则，工具，技巧，以及常见性能问题的解决等。

## 建议1：理解Pythonic概念
1）**`Pythonic`的定义**：充分体现`Python`自身特色的代码风格。

- `The Zen of Python`(`Python`之禅)

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
>>>
```

- 快速排序

```python
def sort_quick(array):
    """
    快速排序
    :param array:
    :return:
    """

    less = list()
    greater = list()
    if len(array) <= 1:
        return array

    pivot = array.pop()
    for index, item in enumerate(array):
        if item <= pivot:
            less.append(item)
        else:
            greater.append(item)

    return sort_quick(less) + [pivot] + sort_quick(greater)
```

2）**代码风格**

- 交换两个变量的值，`packaging/unpackaging`机制

```python
x = 2
y = 3
x, y = y, x
print x, y
```
- 容器遍历

```python
for index, item in enumerate(items):
    do_sth_with(item)
```

- 列表逆序

```python
list_a = [1, 2, 3, 4, 5]
str_c = 'abcdef'
print(list(reversed(list_a)))
print(list(reversed(str_c)))
```

- 标准库

```python
# 字符串格式化
print 'Hello %(name)s!' % {'name': 'Tom'}
```

**注解**：`%`是非常影响可读性的，因为数量多了之后，很难清除哪一个占位符对应哪一个实参。

- `str.format()`：`Python`最为推荐达到字符串格式化方法。

```python
# 字符串格式化, 替代%
print 'Hello {name}!'.format(name='Tom')
```

3）**`Python`的包和模块结构**

> (1) 包和模块的命名采用小写，单数形式且短小；
> (2)包通常作为命名空间，如只包含空的`__init__.py`文件。

## 建议2：编写pythonic代码

1）**要避免劣化代码**

> (1)避免只用大小写来区分不同的对象；
> (2)避免使用容易引起混淆的名称；
> (3)不要害怕过长的变量名。

- 实例1（函数名称，变量名意义均不明）

```python
def funA(list_items, num):
    """
    
    :param list_items: 
    :param num: 
    :return: 
    """
    
    for element in list_items:
        if num == element:
            return True
    
    return False
```

- 实例2（**推荐**）

```python
def find_num(list_search, num):
    """

    :param list_search:
    :param num:
    :return:
    """

    for index, value in enumerate(list_search):
        if num == value:
            return True

    return False
```

2）**`pep8`检测工具**

```shell
C:\>pip install -U pep8
C:\Users\Administrator\Desktop\zxt>pep8 --first database.py
database.py:83:1: E302 expected 2 blank lines, found 1
>pep8 --show-source --show-pep8 waijiao.py
```

3）**深入认识`Python`有助于编写`Pythonic`代码**

> - 掌握`Python`提供的所有特性，包括语言特性和库特性；
> - 跟进学习`Python`的最新版本提供的新特性，掌握其变化趋势；
> - 深入学习公认比较`Pythonic`的代码，例如`Flask`、`gevent`、`requests`等。

## 建议3：理解python与C语言的不同之处

1）**“缩进” 与 “`{}`"**
`Python`中使用严格的代码缩进方式分隔代码块，应养成良好的习惯，统一缩进风格，不要混用`Tab`键和空格。

2）**`'` 与 `"`**
在`C`语言中，二者有严格的区分，但是在`Python`中，区别较小。

```python
Python 2.7.10 (default, Jul 15 2017, 17:16:57) 
>>> str1 = "He said, \"Hello!\""
>>> str2 = 'He said, "Hello!"'
>>> str1
'He said, "Hello!"'
>>> str2
'He said, "Hello!"'
```

3）**三元操作符 `?:`**

```python
x = 0
y = -2
print(x if x < y else y)
-2
```

4）**`switch...case`**

```python
n = raw_input("please input a number:")
if n == "0":
    print "You typed zero."
elif n == "1":
    print "You are in top."
elif n == "2":
    print "N is an even number."
else:
    print "Error!"
```

- 用跳转也可以实现：

```python
def func():
    return {
        "0":  "You typed zero.",
        "1":  "You are in top.",
        "2":  "N is an even number."
    }.get(n, "Error!")
```

## 建议4：在代码中适当添加注释

`Python`有3种形式的代码注释：`块注释`，`行注释`，`文档注释(docstring)`。
> (1）使用块或者行注释的时候仅注释复杂的操作，算法，难以理解的技巧或者不够一目了然的代码；
> (2）注释和代码隔开一定的距离；
> (3）给外部可访问的函数和方法添加文档注释(`docstring`)（`""" """`）；
> (4）推荐文件头部包含`copyright`申明，模块描述等。

```python
"""
Requests HTTP library
~~~~~~~~~~~~~~~~~~~~~
Requests is an HTTP library, written in Python, for human beings. Basic GET
usage:
   >>> import requests
   >>> r = requests.get('https://www.python.org')
   >>> r.status_code
   200
   >>> 'Python is a programming language' in r.content
   True
... or POST:
   >>> payload = dict(key1='value1', key2='value2')
   >>> r = requests.post('http://httpbin.org/post', data=payload)
   >>> print(r.text)
   {
     ...
     "form": {
       "key2": "value2",
       "key1": "value1"
     },
     ...
   }
The other HTTP methods are supported - see `requests.api`. Full documentation
is at <http://python-requests.org>.
:copyright: (c) 2015 by Kenneth Reitz.
:license: Apache 2.0, see LICENSE for more details.
"""
```

## 建议5：通过适当添加空行使代码布局更为优雅，合理

**`Python`代码布局应当遵循以下基本规则**：
1）在一组代码表达完一个完整的思路之后，应该用空白行进行间隔；

- 反例（多余空行）

```python
if guess == number:
    print("Good job!")
    
else:
    print("Nope")
```

2）尽量保持上下文语义的易理解性(如调用函数写在被调用函数之上)；

```python
def A():
    B()


def B():
    pass
```

3）避免过长的代码行，每行最好不要超过80个字符，超过的部分可以用圆括号、方括号、花括号等进行连接，并保存行连接的元素垂直对齐；

```python
x = ('This is a verey long string.'
     'It is used for testing line limited characters')
```

4）不要为了保持水平对齐而使用多余的空格，同时也不要在一行有多个命令；

- 反例（多余的空格）

```python
x =                    5
Year =                 2013
name =                 "Jam"
d2 = {'spam': 2, 'eggs': 3}
```

- 反例（一行中多个命令）

```python
X = 1; Y = 2;
```

5）空格的使用要能在需要强调的时候警示读者：
（1）二元运算符、比较、布尔运算的左右两边应该有空格；

```python
x == 1
```

（2）逗号和分号前不要使用空格；

- 推荐

```python
if x == 4:
    print(x, y)

x, y = y, x
```

- 反例（不推荐）

```python
if x == 4 :
    print(x , y)

x , y = y , x
```

（3）函数名和左右括号之间，序列索引操作时序列名和`[ ]`之间不要空格，函数默认参数两侧不需要空格；

```python
def sort_quick(array, if_print=0):
    ...

arrays = [9, 8, 4, 5, 32, 64, 2, 1, 0, 10, 19, 27]
```

（4）强调前面的操作符的时候使用空格。

```python
-2 - 5
b*b + a*a
```

## 建议6：编写函数的4个原则

**`函数`** 能够带来最大化的代码重用和最小化的代码冗余，不仅可以提高程序的健壮性，还可以增强可读性，减少维护成本。

1）**函数设计尽量短小，嵌套层次不宜过深(最好控制在3层以内)**；

2）**函数声明应该做到合理，简单，易于使用**；

3）**函数参数设计应该考虑向下兼容**；

4）**一个函数只做一件事，尽量保证函数语句粒度的一致性**。

## 建议7：将常量集中到一个文件

`Python`使用常量：
> 通过命名风格来提醒使用者该变量代表的意义为常量，如常量名所有字母大写，用下画线连接各个单词；
> 通过自定义的类实现常量功能。

- 示例：`const.py`

```python
#!/usr/bin/env python
# coding:utf-8


import sys


class _const(object):

    class ConstError(TypeError):
        pass

    class ConstCaseError(ConstError):
        pass

    def __setattr__(self, name, value):
        if self.__dict__.has_key(name):
            raise self.ConstError, "Can't change const.{name}".format(name=name)
        if not name.isupper():
            raise self.ConstCaseError, 'const name "{name}" is not all uppercase'.format(name=name)
        self.__dict__[name] = value


sys.modules[__name__] = _const()
```

- 调用实例

```python
import const


const.COMPANY = "IBM"
print(const.COMPANY)

const.COMPANY = "IBM2"
```

- 上述调用会报错，因为代码中的常量一旦生成便不可更改

```python
Traceback (most recent call last):
  File "/home/projects/pythoner/quality_code/algorithm_sort.py", line 40, in <module>
    const.COMPANY = "IBM2"
  File "/home/projects/pythoner/quality_code/const.py", line 18, in __setattr__
    raise self.ConstError, "Can't change const.{name}".format(name=name)
const.ConstError: Can't change const.COMPANY
```