---
title: 【Python编码规范】编程惯用法
type: categories
copyright: false
date: 2019-04-28 00:09:25
tags: Python编码规范
categories: Python
title_en: python_code91_02
---


> 本系列为《编写高质量代码-改善Python程序的91个建议》的读书笔记。

**温馨提醒**：在阅读本书之前，强烈建议先仔细阅读：[**PEP**规范](https://legacy.python.org/dev/peps/pep-0008/)，增强代码的可阅读性，配合优雅的[pycharm](http://www.jetbrains.com/pycharm/)编辑器(开启`pep8`检查)写出规范代码，是`Python`入门的第一步。

## 建议8：利用assert语句来发现问题

- 断言(`assert`)基本语法如下：

```python
assert expression1 ["," expression2]
```

- 	`assert`用法举例：

```python
>>> x = 1
>>> y = 2
>>> assert x == y , "not equals"
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AssertionError: not equals
```

- 关于`assert`的几点说明事项

> 1）`__debug__`的值默认为`True`，且只读，无法修改(`Python2.7`)。
> 2）断言是有代价的，对性能产生一定影响。禁用断言的方法是在运行脚本的时候加上`-O`标记(不优化字节码，而是忽略与断言相关的语句)。

- 使用断言(`assert`)注意点：

> 1）不要滥用，这是使用断言**最基本**的原则；
> 2）如果`Python`本身的异常能够处理就不要再使用断言；
> 3）不要使用断言来检查用户的输入；
> 4）在函数调用后，当需要确认返回值是否合理时可以使用断言；
> 5）当条件时业务逻辑继续下去的先决条件时，可以使用断言。

## 建议9：数据交换值时不推荐使用中间交换变量

```python
>>> from timeit import Timer
>>> Timer('temp=x;x=y;y=temp','x=2;y=3').timeit()
0.03472399711608887
>>> Timer('x,y=y,x','x=2;y=3').timeit()
0.031581878662109375
```

- **测试用例说明**：不借助中间变量的方式耗费的时间更少，代码简洁，值得推荐。
 
## 建议10：充分利用Lazy evaluation的特性

`Lazy evaluation`常被译作“`延时计算`”或“`惰性计算`”，指的是仅仅在真正需要执行的时候才计算表达式的值。**典型例子**：生成器表达式。
> 1）避免不必要的计算，带来性能上的提升；
> 2）节省空间，使用无限循环的数据结构成为可能。

- 实例：

```python
#!/usr/bin/env python
# coding:utf-8


from itertools import islice


def fib():
    a, b = 0, 1
    while True:
        yield a

        a, b = b, a+b


if __name__ == '__main__':

    print(list(islice(fib(), 5)))
```

## 建议11：理解枚举替代实现的缺陷

1）**替代方法**

- 使用类属性

```python
>>> class Seasons(object):
...     Spring, Summer, Autumn, Winter = xrange(4)
... 
>>> print(Seasons.Spring)
0
```

- 借助函数

```python
>>> def enum(*args, **kwargs):
...     return type("Enum", (object,), dict(zip(args, xrange(len(args))), **kwargs))
... 
>>> Seasons = enum("Spring", "Summer", "Autumn", Winter=3)
>>> Seasons.Summer
1
>>> Seasons.Winter
3
```

- 使用`collections.namedtuple`

```python
>>> from collections import namedtuple
>>> Seasons = namedtuple('Seasons','Spring Summer Autumn Winter')._make(xrange(4)) 
>>> print Seasons
Seasons(Spring=0, Summer=1, Autumn=2, Winter=3)
>>> print Seasons.Autumn
2
```

2）**替代缺陷**

- 允许枚举值重复

```python
>>> from collections import namedtuple
>>> Seasons = namedtuple('Seasons','Spring Summer Autumn Winter')._make(xrange(4)) 
>>> Seasons
Seasons(Spring=0, Summer=1, Autumn=2, Winter=3)
>>> Seasons._replace(Spring=2)   # 不合理
Seasons(Spring=2, Summer=1, Autumn=2, Winter=3)
```

- 支持无意义的操作

```python
>>> Seasons.Summer + Seasons.Autumn == Seasons.Winter    # 无意义
True
```

3）**`Python2.7`的替代方案(`Python3.4`后引入`Enum`类型)**：**`flufl.enum`**

```python
#!/usr/bin/env python
# coding:utf-8


from flufl.enum import Enum
​
​
class Seasons(Enum):
​
    Spring = "Spring"
    Summer = 2
    Autumn = 3
    Winter = 4
​
​
Seasons = Enum('Seasons', 'Spring Summer Autumn Winter')
​
print Seasons
print Seasons.Summer.value
```

## 建议12：不推荐使用type来进行类型检查

1）基于内建类型扩展的用户自定义类型，`type`函数并不能准确返回结果。

```python
#!/usr/bin/env python
# coding:utf-8


import types


class UserInt(int):
    """
    用户类UserInt继承int类实现定制化，不支持操作符（+=）
    """

    def __init__(self, value=0):
        self._value = int(value)

    def __add__(self, other):
        if isinstance(other, UserInt):
            return UserInt(self._value + other._value)

        return self._value + other

    def __iadd__(self, other):
        raise NotImplementedError("not support operation.")

    def __str__(self):
        return str(self._value)

    def __repr__(self):
        return "Integer({0})".format(self._value)


if __name__ == '__main__':

    n = UserInt()
    print(n)           # 输出 0

    m = UserInt(2)
    print(m)           # 输出 2

    print(n+m)         # 输出 2
    print(type(n) is types.IntType)  # 使用type进行类型判断，输出 False
    print(isinstance(n, int))  # 输出 True 
```

2）在旧式类中，所有类的实例的`type`值都相等。

```python
>>> class A:
...     pass
... 
>>> a = A()
>>> class B:
...     pass
... 
>>> b = B()
>>> type(a) == type(b)
True
>>> type(a)
<type 'instance'>
```

3）可以用`isinstance()`函数检查。

```python
>>> isinstance(2, float)
False
>>> isinstance("a", (str, unicode))
True
>>> isinstance((2,3), (str, list))
False
>>> isinstance((2,3), (str, list, tuple))
True
```

## 建议13：尽量转换为浮点类型再做除法

**当涉及除法运算的时候尽量先将操作数转换成浮点类型再做运算。**

- 浮点数不精确性导致的无限循环：

```python
>>> i=1
>>> while i!=1.5:
...     i=i+0.1
...     print i
```

## 建议14：警惕eval()的安全漏洞

- 实例：根据用户的输入，计算`Python`表达式的值

```python
# -*-coding:UTF-8 -*-
​
​
import sys
from math import *
​
def ExpCalcBot(string):
    try:
        print "Your answer is", eval(string)
    except NameError:
        print "The expression you enter is not valid."
​
while True:
    print 'Please enter a number or operation. Enter e to complete. '
    inputStr = raw_input()
    if inputStr == 'e':
        sys.exit()
    elif repr(inputStr) != ' ':
        ExpCalcBot(inputStr)
```

**输入**：`__import__("os").system("dir")`：显示当前目录下的所有文件；
				`__import__("os").system("del */Q")`：删除当前目录下的所有文件。

因此，在实际应用过程中，**如果使用对象不是信任源，应该尽量避免使用`eval`，在需要使用`eval`的地方可以用安全性更好的`ast.literal_eval`替代**。

## 建议15：使用enumerate()获取序列迭代的索引和值
	
对序列进行迭代并获取序列中的元素进行处理的几种方法举例：

- **方法一**  在每次循环中对索引变量进行自增

```python
li = ['a', 'b', 'c', 'd', 'e']
index = 0
for i in li:
    print("index:", index, "element:", i)
    index += 1
```

- **方法二** 使用`range()`和`len()`方法结合

```python
li = ['a', 'b', 'c', 'd', 'e']
for i in xrange(len(li)):
    print("index:", i, "element:", li[i])
```

- **方法三** 使用`while`循环，用`len`获取循环次数

```python
li = ['a', 'b', 'c', 'd', 'e']
i = 0
while i < len(li):
    print("index:", i, "element:", li[i])
    i += 1
```

- **方法四** 使用`zip()`方法

```python
li = ['a', 'b', 'c', 'd', 'e']
for i, e in zip(range(len(li)), li):
    print("index:", i, "element:", e)
```

- **方法五(推荐)** 使用`enumerate()`获取序列迭代对索引和值

```python
li = ['a', 'b', 'c', 'd', 'e']
for i, e in enumerate(li):
    print("index:", i, "element:", e)
```

**`注意`**：在获取迭代过程中字典的`key`和`value`，应该使用如下`iteritems()`方法(`Python3`不再适用)。

```python
>>> person={'name': 'Josn', 'age': 19, 'hobby': 'football'}
>>> for k,v in person.iteritems():
...     print k, ":", v
```

## 建议16：分清`==`与`is`的适用场景

```python
>>> a="Hi"
>>> b="Hi"
>>> a is b
True
>>> a==b          # is 和 == 结果是一样的
True
>>> a1 ="I am using long string for testing"         # 注意区分
>>> b1 ="I am using long string for testing"
>>> a1 is b1
False
>>> a1==b1                # is 和 == 结果是不一样的
True
```

![is与==的区别](/images/python_code91_02_idis_20190428.png)

**`is`**：即`object identity`，表示的是对象标识符，检查对象的标识符是否一致，也就是比较两个对象在内存中是否拥有同一块内存空间；

**`==`**：即`equal`，表示的是值相等，用来判断两个对象的值是否相等，可以被重载。

**字符串驻留(`string interning`)机制**：对于较小的字符串，为了提高系统性能会保留其值的一个副本，当创建新的字符串时直接指向该副本即可。

**注意**：判断两个对象相等应该使用 `==` 而不是 `is`。
 
## 建议17：考虑兼容性，尽可能使用Unicode

`Python`内建的字符串有两种类型：`str`和`Unicode`，共同祖先为`basestring`。

```python
>>> str_uni = u'unicode字符串'  # 前面加u表示Unicode
>>> str_uni
u'unicode\u5b57\u7b26\u4e32'
>>> print(str_uni)
unicode字符串
>>> type(str_uni)
<type 'unicode'>
>>> type(str_uni).__bases__
(<type 'basestring'>,)
```

- `Unicode`：又称`万国码`，为每种语言设置了唯一的二进制编码表示方式，提供从数字代码到不同语言字符集之间的映射，从而满足跨平台、跨语言之间的文本处理要求。

- `Unicode`编码系统分为**编码方式**和**实现方式**
> - 在编码方式上，分为`UCS-2`和`UCS-4`，`UCS-2`用两个字节编码；`UCS-4`用四个字节编码。
> - 实现方式又称为`Unicode`转换方式，简称`UTF`，包括`UTF-7`、`UTF-8`、`UTF-16`、`UTF-32`等。
> - `UTF-8` 较为常见，其特点是对不同范围的字符使用不同长度的编码，其中`0x00～0x7F`的字符`UTF-8`编码与`ASCII`编码完全相同；其最大长度是4个字节。

![UTF-8编码方式](/images/python_code91_02_methodcoding_20190428.png)
- `Windows`本地默认编码是`CP936`。

> - 解码：`str.decode([编码参数[，错误处理]])`  
> - 编码：`str.encode([编码参数[，错误处理]])`
> 错误处理参数有3种方式：
>>（1）`strict`：默认值，抛出`UnicodeError`异常；
>>（2）`ignore`：忽略不可转换的字符；
>>（3）`replace`：将不可转换字符用`?`代替。

- 常见的编码参数

![常见的编码参数](/images/python_code91_02_codingargs_20190428.png)

- 对于A、B两种编码系统之间的相互转换示意图如下：

![编码转换示意图](/images/python_code91_02_utf8coding_20190428.png)

- 有些软件在保存`UTF-8`编码时，会在文件最开始地方插入不可见的`BOM`(`0xEF`，`0xBB`，`0xBF`，	即`BOM`)，可以利用`codecs`模块解决。

```python
import codecs
​
content = open('manage.py', 'r').read()
if content[:3] == codecs.BOM_UTF8:
    content = content[:3]
print content.decode("utf-8")
 ```
 
 - 编码声明的三种方式：
 
```python
# coding=<encoding name>           #方式一
​
​
#!/usr/bin/env python
# -*- coding:<encoding name> -*-    #方式二
​
​
#!/usr/bin/env python
# vim:set fileencoding=<encoding name>    #方式三
```

## 建议18：构建合理的包层次来管理module

本质上，每一个`Python`文件都是一个模块，使用模块可以增强代码的可维护性和可重用性。

**`包`**	即目录，包含一个`__init__.py`文件，允许嵌套。包中的模块通过“**`.`**”访问符进行访问，即“**包名.模块名**”。

- 直接导入一个包

```python
import package
```

- 导入子模块或者子包，包嵌套的情况下可以进行嵌套导入

```python
from package import module
import package.module

from package import subpackage
import package.subpackage
from package.subpackage import module
import package.subpackage.module
```

- 包中`__init__.py`文件的作用
> 1）使包和普通目录区分；
> 2）在该文件中声明模块级别的`import`语句，从而使其变成包级别可见；
> 3）通过该文件中定义`__all__`变量，控制需要导入的子包或者模块。

- 使用包的好处
> 1）合理组织代码，便于维护和使用；
> 2）能够有效地避免名称空间冲突。