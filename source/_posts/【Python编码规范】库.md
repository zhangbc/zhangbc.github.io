---
title: 【Python编码规范】库
type: categories
copyright: false
date: 2019-05-12 23:01:16
tags: Python编码规范
categories: Python
title_en: python_code91_04
---


> 本系列为《编写高质量代码-改善Python程序的91个建议》的读书笔记。

**温馨提醒**：在阅读本书之前，强烈建议先仔细阅读：[**PEP**规范](https://legacy.python.org/dev/peps/pep-0008/)，增强代码的可阅读性，配合优雅的[pycharm](http://www.jetbrains.com/pycharm/)编辑器(开启`pep8`检查)写出规范代码，是`Python`入门的第一步。

## 建议36：掌握字符串的基本用法
 
**`Python`小技巧**：`Python`遇到未闭合的小括号会自动将多行代码拼接为一行和把相邻的两个字符串字面量拼接在一起的。

```python
>>> st = ('select * '
...       'from table '
...       'whre field="value";')
>>> st
'select * from table whre field="value";'
```

- 字符串用法举例：

```python
>>> print isinstance('hello world', basestring)  # basestring是str与unicode的基类
True
>>> print isinstance('hello world', unicode)
False
>>> print isinstance('hello world', str)
True
>>> print isinstance(u'hello world', unicode)
True
```

- `split()`的陷阱示例

```python
>>> ' Hello World'.split(' ')
['', 'Hello', 'World']
>>> ' Hello   World'.split()
['Hello', 'World']
>>> ' Hello   World'.split(' ')
['', 'Hello', '', '', 'World']
```

- `title()`应用示例

```python
>>> import string
>>> string.capwords('hello  wOrld')
'Hello World'
>>> string.capwords(' hello  wOrld ')
'Hello World'
>>> ' hello  wOrld '.title()
' Hello  World '
```

## 建议37：按需选择sort()或者sorted()

`sorted(iterable[, cmp[, key[, reverse]]])`：作用于任何可迭代对象，返回一个排序后的列表；

`sort(cmp[, key[, reverse]]])`：一般作用于列表，直接修改原有列表，返回为`None`。
 
1）**对字典进行排序**

```python
>>> from operator import itemgetter
>>> phone_book = {'Linda': '775', 'Bob': '9349', 'Carol': '5834'}
>>> sorted_pb = sorted(phone_book.iteritems(), key=itemgetter(1))  ​# 按照字典的value进行排序
>>> print phone_book
{'Linda': '775', 'Bob': '9349', 'Carol': '5834'}
>>> print sorted_pb
[('Carol', '5834'), ('Linda', '775'), ('Bob', '9349')]
 ```
 
 2）**多维`list`排序**
​
```python
>>> from operator import itemgetter
>>> game_result = [['Linda', 95, 'B'], ['Bob', 93, 'A'], ['Carol', 69, 'D'], ['zhangs', 95, 'A']]
>>> sorted_res = sorted(game_result, key=itemgetter(1, 2))  # 按照学生成绩排序，成绩相同的按照等级排序
>>> print game_result
[['Linda', 95, 'B'], ['Bob', 93, 'A'], ['Carol', 69, 'D'], ['zhangs', 95, 'A']]
>>> print sorted_res
[['Carol', 69, 'D'], ['Bob', 93, 'A'], ['zhangs', 95, 'A'], ['Linda', 95, 'B']]
```

3）**字典中混合`list`排序**

```python
>>> from operator import itemgetter
>>> list_dict = {
...     'Li': ['M', 7],
...     'Zhang': ['E', 2],
...     'Du': ['P', 3],
...     'Ma': ['C', 9],
...     'Zhe': ['H', 7]
... }
>>> sorted_ld = sorted(list_dict.iteritems(), key=lambda (k, v): itemgetter(1)(v))  # 按照字典的value[m,n]中的n值排序
>>> print list_dict
{'Zhe': ['H', 7], 'Zhang': ['E', 2], 'Ma': ['C', 9], 'Du': ['P', 3], 'Li': ['M', 7]}
>>> print sorted_ld
[('Zhang', ['E', 2]), ('Du', ['P', 3]), ('Zhe', ['H', 7]), ('Li', ['M', 7]), ('Ma', ['C', 9])]
```

4）**`list`中混合字典排序**

```python
>>> from operator import itemgetter
>>> game_result = [
...     {'name': 'Bob', 'wins': 10, 'losses': 3, 'rating': 75},
...     {'name': 'David', 'wins': 3, 'losses': 5, 'rating': 57},
...     {'name': 'Carol', 'wins': 4, 'losses': 5, 'rating': 57},
...     {'name': 'Patty', 'wins': 9, 'losses': 3, 'rating': 71.48}
... ]
>>> sorted_res = sorted(game_result, key=itemgetter('rating', 'name'))   # 按照name和rating排序
>>> print game_result
[{'wins': 10, 'losses': 3, 'name': 'Bob', 'rating': 75}, {'wins': 3, 'losses': 5, 'name': 'David', 'rating': 57}, {'wins': 4, 'losses': 5, 'name': 'Carol', 'rating': 57}, {'wins': 9, 'losses': 3, 'name': 'Patty', 'rating': 71.48}]
>>> print sorted_res
[{'wins': 4, 'losses': 5, 'name': 'Carol', 'rating': 57}, {'wins': 3, 'losses': 5, 'name': 'David', 'rating': 57}, {'wins': 9, 'losses': 3, 'name': 'Patty', 'rating': 71.48}, {'wins': 10, 'losses': 3, 'name': 'Bob', 'rating': 75}]
```

## 建议38：使用copy模块深拷贝对象

- `浅拷贝(shallow copy)`：构造一个新的复合对象并将从原对象中发现的引用插入该对象中。实现方式有：工厂函数，切片操作，`copy`模块中`copy`操作等；
- `深拷贝(deep copy)`：构造一个新的复合对象，但是遇到引用会继续递归拷贝其所指向的具体内容，也就是说它会针对引用所指向的对象继续进行拷贝，因此产生的对象不受其他引用对象操作的影响。实现方式有`copy`模块中的`deepcopy()`操作。

- 实例

```python
#!/usr/bin/env python
# coding:utf-8


from copy import copy
from copy import deepcopy


class Pizza(object):
    """

    """

    def __init__(self, name, size, price):
        self.name = name
        self.size = size
        self.price = price

    def get_pizza_info(self):
        return self.name, self.size, self.price

    def show_pizza_info(self):
        print "Pizza name: {0}, size: {1}, price: {2}".format(self.name, self.size, self.price)

    def change_size(self, size):
        """

        :param size:
        :return:
        """

        self.size = size

    def change_price(self, price):
        """

        :param price:
        :return:
        """

        self.price = price


class Order(object):
    """

    """

    def __init__(self, name):
        self.customer_name = name
        self.pizza_list = list()
        self.pizza_list.append(Pizza("Mushroom", 12, 30))

    def order_more(self, pizza):
        """

        :param pizza:
        :return:
        """

        self.pizza_list.append(pizza)

    def change_name(self, name):
        """

        :param name:
        :return:
        """

        self.customer_name = name

    def get_oder_detail(self):
        """

        :return:
        """

        print "Customer name: {0}".format(self.customer_name)
        for index, item in enumerate(self.pizza_list):
            item.show_pizza_info()

    def get_pizza(self, number):
        """

        :param number:
        :return:
        """

        return self.pizza_list[number]


def customer_one():
    c1 = Order("zhang San")
    c1.order_more(Pizza("seafood", 9, 40))
    c1.order_more(Pizza("fruit", 12, 35))
    print "==============Customer one order info================="
    c1.get_oder_detail()

    c2 = copy(c1)
    print "==============Customer two order info(copy)================="
    c2.change_name("Li Si")
    c2.get_pizza(2).change_size(9)
    c2.get_pizza(2).change_price(30)
    c2.get_oder_detail()

    c3 = deepcopy(c1)
    print "==============Customer three order info(deepcopy)================="
    c3.change_name("Li Si")
    c3.get_pizza(1).change_size(10)
    c3.get_pizza(1).change_price(50)
    c3.get_oder_detail()

    print "==============Customer one order info================="
    c1.get_oder_detail()


if __name__ == '__main__':
    customer_one()
```

- 运行结果如下：

```python
==============Customer one order info=================
Customer name: zhang San
Pizza name: Mushroom, size: 12, price: 30
Pizza name: seafood, size: 9, price: 40
Pizza name: fruit, size: 12, price: 35
==============Customer two order info(copy)=================
Customer name: Li Si
Pizza name: Mushroom, size: 12, price: 30
Pizza name: seafood, size: 9, price: 40
Pizza name: fruit, size: 9, price: 30
==============Customer three order info(deepcopy)=================
Customer name: Li Si
Pizza name: Mushroom, size: 12, price: 30
Pizza name: seafood, size: 10, price: 50
Pizza name: fruit, size: 9, price: 30
==============Customer one order info=================
Customer name: zhang San
Pizza name: Mushroom, size: 12, price: 30
Pizza name: seafood, size: 9, price: 40
Pizza name: fruit, size: 9, price: 30
```

## 建议39：使用Counter进行计数统计

- 使用`dict`

```python
>>> some_data = ['a', 2, '2', 4, 5, '2', 'b', 7, 'a', 5, 'd', 'a', 'z']
>>> count_frq = dict()
>>> for index, item in enumerate(some_data):
...     if item in count_frq:
...         count_frq[item] += 1
...     else:
...         count_frq[item] = 1
...         
>>> print count_frq
{'a': 3, 2: 1, 'b': 1, 4: 1, 5: 2, 7: 1, '2': 2, 'z': 1, 'd': 1}
```

- 使用`defaultdict`

```python
>>> from collections import defaultdict
>>> some_data = ['a', 2, '2', 4, 5, '2', 'b', 7, 'a', 5, 'd', 'a', 'z']
>>> count_frq = defaultdict(int)
>>> for index, item in enumerate(some_data):
...     count_frq[item] += 1
...     
>>> print count_frq
defaultdict(<type 'int'>, {'a': 3, 2: 1, 'b': 1, 4: 1, 5: 2, 7: 1, '2': 2, 'z': 1, 'd': 1})

```

- 使用`set`与`list`

```python
>>> some_data = ['a', 2, '2', 4, 5, '2', 'b', 7, 'a', 5, 'd', 'a', 'z']
>>> count_set = set(some_data)
>>> count_list = list()
>>> for index, item in enumerate(some_data):
...     count_list.append((item, some_data.count(item)))
...     
>>> print count_list
[('a', 3), (2, 1), ('2', 2), (4, 1), (5, 2), ('2', 2), ('b', 1), (7, 1), ('a', 3), (5, 2), ('d', 1), ('a', 3), ('z', 1)]
```

- 使用更为优雅的`Pythonic`方法--`collections.Counter`

```python
>>> from collections import Counter
>>> some_data = ['a', 2, '2', 4, 5, '2', 'b', 7, 'a', 5, 'd', 'a', 'z']
>>> print Counter(some_data)
Counter({'a': 3, 5: 2, '2': 2, 2: 1, 'b': 1, 4: 1, 7: 1, 'z': 1, 'd': 1})
>>> print Counter('success')
Counter({'s': 3, 'c': 2, 'e': 1, 'u': 1})
>>> print Counter(s=3, c=2, e=1, u=1)
Counter({'s': 3, 'c': 2, 'u': 1, 'e': 1})
>>> print Counter({'s': 3, 'c': 2, 'u': 1, 'e': 1})
Counter({'s': 3, 'c': 2, 'u': 1, 'e': 1})
>>> print list(Counter(some_data).elements())
['a', 'a', 'a', 2, 'b', 4, 5, 5, 7, '2', '2', 'z', 'd']
>>> print Counter(some_data).most_common(3) # 出现频次最高的前三个字符
[('a', 3), (5, 2), ('2', 2)]
```

## 建议40：深入理解ConfigParser

- 实例

```python
#!/usr/bin/env python
# coding:utf-8


import ConfigParser


conf = ConfigParser.ConfigParser()
conf.read('config.ini')
print conf.get('default', 'host')


conf = ConfigParser.ConfigParser()
conf.read('config.ini')
print conf.get('online', 'conn_str')   # 仅在default下

​
====================config.ini=========================

[default]
conn_str = %(dbn)s://%(user)s:%(pw)s@%(host)s:%(port)s/%(db)s
dbn = msyql
host = 127.0.0.1
user = root
port = 3306
pw = xxxxxx
db = test

[online]
conn_str = %(dbn)s://%(user)s:%(pw)s@%(host)s:%(port)s/%(db)s
dbn = msyql
host = 127.0.0.1
user = root
port = 3306
pw = xxxxxx
db = test
```

## 建议41：使用argparese处理命令行参数

```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument('-v', dest='verbose', action='store_true')
args = parser.parse_args()
print args
```

## 建议42：使用pandas处理大型csv文件

`csv`作为一种逗号分隔型值的纯文本格式文件，常见于数据库数据的导入导出、数据分析中记录的存储等。

以下列举几个与`csv`处理相关的`API`：

- `csv.reader(csvfile[, dialect='excel'][, fmtparam])`：用于`CSV`文件的读取，返回一个`reader`对象用于在`CSV`文件中进行行迭代；
- `csv.writer(csvfile, dialect='excel', **fmtparams)`：用于写入`CSV`文件；
- `csv.DictReader(csvfile, fieldnames=None, restKey='', restval='', dialect='excel', *args, **kwds)`：用于支持字典的读取；
- `csv.DictReader(csvfile, fieldnames=None, restval='', extrasaction='raise', dialect='excel', *args, **kwds)`：用于支持字典的写入。

- 实例

```python
import csv


# 写入csv
with open('csv_test.csv', 'wb') as fp:
    fields = ['Tran_date', 'Product', 'Price', 'PaymentType']
    writer = csv.DictWriter(fp, fieldnames=fields)
    writer.writerow(dict(zip(fields, fields)))
    data = {'Tran_date': '1/2/09 6:17',
            'Product': 'Nick',
            'Price': '1200',
            'PaymentType': 'Mastercard'}
    writer.writerow(data)


# 读取csv
with open('csv_test.csv', 'rb') as fp:
    for item in csv.DictReader(fp):
        print item
```

`csv`使用非常简单，基本可以满足大部分需求，但是对于上百`MB`或`G`级别以上的文件处理无能为力。这种情况下，可以考虑使用`pandas`模块，它支持以下两种数据结构。

- `Series`：是一种类似数组的带索引的一维数据结构，支持的类型与`NumPy`兼容。

```python
>>> from pandas import Series
>>> obj = Series([1, 'a', (1, 2), 3], index=['a', 'b', 'c', 'd'])
>>> obj
a         1
b         a
c    (1, 2)
d         3
dtype: object
>>> obj_dic = Series({'Book': 'Python', 'Author': 'Dan', 'ISBN': '011334', 'Price': 25}, index=['book', 'Author', 'ISBN', 'Price'])
>>> obj_dic
book         NaN  # 匹配失败，导致数据丢失
Author       Dan
ISBN      011334
Price         25
dtype: object
>>> obj_dic.isnull()
book       True
Author    False
ISBN      False
Price     False
dtype: bool
```

- `DataFrame`：类似于电子表格，其数据为排好序的数据列的集合，每一列都可以是不同的数据类型，类似一个二维数组，支持行和列的索引。

```python
>>> from pandas import DataFrame
>>> data = {'OrderDate': ['1-6-10', '1-23-10', '2-9-10', '2-26-10', '3-15-10'],
...         'Region': ['East', 'Central', 'Central', 'West', 'East'],
...         'Rep': ['Jones', 'Kivell', 'Jardine', 'Gill', 'Sorvino']}
>>> DataFrame(data, columns=['OrderDate', 'Region', 'Rep'])
  OrderDate   Region      Rep
0    1-6-10     East    Jones
1   1-23-10  Central   Kivell
2    2-9-10  Central  Jardine
3   2-26-10     West     Gill
4   3-15-10     East  Sorvino
```

`pandas`中处理`CSV`文件的函数主要为`read_csv()`和`to_csv()`。

- 指定读取部分列和文件的行数

```python
>>> import pandas as pd
>>> df = pd.read_csv('/home/projects/pythoner/quality_code/csv_test.csv', nrows=5, usecols=['Tran_date', 'Product', 'Price'])
>>> df
     Tran_date Product  Price
0  1/2/09 6:17    Nick   1200
1  2/2/09 6:17    Nick   1200
2  3/2/09 6:17    Nick   1200
3  4/2/09 6:17    Nick   1200
4  5/2/09 6:17    Nick   1200
```

- 设置`CSV`文件与`excel`兼容

```python
>>> import csv
>>> import pandas as pd
>>> dia = csv.excel()
>>> dia.delimiter = ","
>>> pd.read_csv('/home/projects/pythoner/quality_code/csv_test.csv', dialect=dia, error_bad_lines=False)
      Tran_date Product  Price PaymentType
0   1/2/09 6:17    Nick   1200  Mastercard
1   2/2/09 6:17    Nick   1200  Mastercard
2   3/2/09 6:17    Nick   1200  Mastercard
3   4/2/09 6:17    Nick   1200  Mastercard
4   5/2/09 6:17    Nick   1200  Mastercard
5   6/2/09 6:17    Nick   1200  Mastercard
6   7/2/09 6:17    Nick   1200  Mastercard
7   8/2/09 6:17    Nick   1200  Mastercard
8   9/2/09 6:17    Nick   1200  Mastercard
9  10/2/09 6:17    Nick   1200  Mastercard
```

- 对文件进行分块处理并返回一个可迭代的对象

```python
>>> reader = pd.read_table('/home/projects/pythoner/quality_code/csv_test.csv', chunksize=5, iterator=True)
>>> iter(reader).next()
  Tran_date,Product,Price,PaymentType
0    1/2/09 6:17,Nick,1200,Mastercard
1    2/2/09 6:17,Nick,1200,Mastercard
2    3/2/09 6:17,Nick,1200,Mastercard
3    4/2/09 6:17,Nick,1200,Mastercard
4    5/2/09 6:17,Nick,1200,Mastercard
>>> iter(reader).next()
  Tran_date,Product,Price,PaymentType
5    6/2/09 6:17,Nick,1200,Mastercard
6    7/2/09 6:17,Nick,1200,Mastercard
7    8/2/09 6:17,Nick,1200,Mastercard
8    9/2/09 6:17,Nick,1200,Mastercard
9   10/2/09 6:17,Nick,1200,Mastercard
```

- 当文件格式相似时，支持多个文件合并处理

```python
>>> file_list = ['/home/projects/pythoner/quality_code/csv_test1.csv', '/home/projects/pythoner/quality_code/csv_test2.csv']
>>> dfs = [pd.read_csv(f) for f in file_list]
>>> total_df = pd.concat(dfs)
>>> total_df
      Tran_date Product  Price PaymentType
0   1/2/09 6:17    Nick   1200  Mastercard
1   2/2/09 6:17    Nick   1200  Mastercard
2   3/2/09 6:17    Nick   1200  Mastercard
3   4/2/09 6:17    Nick   1200  Mastercard
4   5/2/09 6:17    Nick   1200  Mastercard
0   6/2/09 6:17    Nick   1200  Mastercard
1   7/2/09 6:17    Nick   1200  Mastercard
2   8/2/09 6:17    Nick   1200  Mastercard
3   9/2/09 6:17    Nick   1200  Mastercard
4  10/2/09 6:17    Nick   1200  Mastercard
```

## 建议43：一般情况使用ElementTree解析XML

`ElementTree`解析`XML`具有以下特性：

- 使用简单，将整个`XML`文件以树的形式展示，每一个元素的属性以字典的形式表示，非常方便处理；
- 内存上消耗明显低于`DOM`解析；
- 支持`XPath`查询，非常方便获取任意结点的值。

## 建议44：理解模块pickle优劣

1）`pickle.dump(obj,file[,protocol])`：序列化数据到一个文件描述符。 其中：`protocol`为序列化使用的协议版本，0表示`ASCII`协议，为默认值；1表示老式的二进制协议；2表示2.3版本引入的新二进制协议。

2）`pickle.load()`：表示把文件中的对象恢复为原来的对象，这个过程也被称为`反序列化`。

```python
import cPickle as pickle
​
​
my_data = {"name": "Python", "type": "language", "version": "2.7.6"}
fp = open('pickle.dat', 'wb')
pickle.dump(my_data, fp)
fp.close()
​
fp = open('pickle.dat', 'rb')
out = pickle.load(fp)
print out
fp.close()
```

- `pickle`的优点：

> 1）接口简单，容易使用；
> 2）`pickle`的存储格式具有通用性，能够被不同平台的`Python`解析器共享；
> 3）支持的数据类型广泛；
> 4）`pickle`模块是可扩展的；
> 5）能够自动维护对象间的引用。

- `pickle`的缺点：

> 1）`pickle`不能保证操作的原子性；
> 2）`pickle`存在安全性问题；
> 3）`pickle`协议是`Python`特定的，不同语言之间的兼容性难以保证。

## 建议45：序列化的另一个不错的选择--JSON

`JSON`具有以下优势：

- 使用简单，支持多种数据类型；仅存在两大数据结构：名称/值对的集合（对象，记录，结构，字典，散列表，键列表，关联数组等）；值的有序列表（数组，向量，列表，序列等）。
- 存储格式可读性好，容易修改；
- `json`支持跨平台跨语言操作，能够轻易被其他语言解析，存储格式较紧凑，所占空间较小；
- 具有较强的扩展性；
- `json`在序列化`datetime`时会抛出`TypeError`异常，需要对`json`本身的`JSONEncoder`进行扩展。

## 建议46：使用traceback获取栈信息

- 实例

```python
import trackback


g_list = ['a', 'b', 'c', 'd', 'e', 'f', 'g']


def f():
    g_list[5]
    return g()


def g():
    return h()


def h():
    del g_list[2]
    return i()


def i():
    g_list.append('i')
    print g_list[7]


if __name__ == '__main__':

    try:
        f()
    except IndexError as e:
        print 'Error: {0}'.format(e)
        traceback.print_exc()
```

输出结果如下：

```python
Error: list index out of range
Traceback (most recent call last):
  File "/home/projects/pythoner/quality_code/configure_parser.py", line 33, in <module>
    f()
  File "/home/projects/pythoner/quality_code/configure_parser.py", line 13, in f
    return g()
  File "/home/projects/pythoner/quality_code/configure_parser.py", line 17, in g
    return h()
  File "/home/projects/pythoner/quality_code/configure_parser.py", line 22, in h
    return i()
  File "/home/projects/pythoner/quality_code/configure_parser.py", line 27, in i
    print g_list[7]
IndexError: list index out of range
```

## 建议47：使用logging记录日志信息

1，**日志级别**

| Level           | 使用情形|
|:----------------|:--|
| DEBUG      | 详细的信息，在追踪问题时使用 |
| INFO          | 正常的信息 |
| WARNING | 一些不可预见的问题发生，或者将要发生，如磁盘空间低等，但不影响程序的运行 |
| ERROR     | 由于某些严重的问题，程序中的一些功能受到影响 |
| CRITICAL  | 严重的错误，或者程序本身不能继续运行 |

2， **`logging lib`的四个主要对象**

- **`logger`**：程序信息输出的接口，分散在不同的代码中，使得程序可以在运行时记录相应的信息，并根据设置的日志级别或者`filter`来决定哪些信息需要输出，并将这些信息分发到其关联的`handler`。
- **`Handler`**：用来处理信息的输出，可以将信息输出到控制台、文件或者网络。
- **`Formatter`**：决定`log`信息的格式。
- **`Filter`**：决定哪些信息需要输出，可以被`handler`和`logger`使用，支持层次关系。

`logging.basicConfig([**kwargs])` 提供对日志系统的基本配置，默认使用`StreamHandler`和`Formatter`并添加到`root logger`。字典参数列表如下：

| 格式       | 描述                                                                                     | 
|:------------|:---------------------------------------------------------------------------- |
| filename | 指定FileHandler的文件名，而不是默认的StreamHandler  |
| filemode | 打开文件的模式，默认为‘a’                                                |
| format    | 输出格式字符串                                                                   |
| datefmt  | 日期格式                                                                              |
| level       | 设置root logger的日志级别                                                  |
| stream   | 指定StreamHandler，若与filename冲突，忽略stream        |

- 实例

```python
def get_logger(file_name, level=logging.INFO):
    """
    设置日志文件输出
    :param file_name: 文件名称
    :param level: 日志严重级别 ==> CRITICAL > ERROR > WARNING > INFO > DEBUG > NOTSET
    :return:
    """
    logger = logging.getLogger()
    logger.setLevel(level)
    file_handler = logging.FileHandler(file_name, encoding="utf-8")
    file_handler.setLevel(level)
    formatter = logging.Formatter("%(asctime)s %(name)s %(levelname)s [line %(lineno)s]: %(message)s")
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)
```

3，**使用建议**

> 1）尽量为`logging`取一个名字而不是采用默认，`eg`：`logger=logging.getLogger(__name__)；`
> 2）为了方便找出问题，`logging`的名字建议以模块或者`class`命名；
> 3）`logging`只是线程安全的，不支持多进程写入同一个日志文件。

## 建议48：使用threading模块编写多线程程序

- `Python`多线程支持两种方式创建：

> 1）通过继承`Thread`类，重写其`run()`方法(不是`start()`方法)；不支持守护线程；
> 2）创建`threading.Thread`对象,在它的初始化函数（`__init__()`）中将可调用对象作为参数传入。

## 建议49：使用Queue使多线程编程更加安全

`Python`中的`Queue`模块提供了以下队列：

- **`Queue.Queue(maxsize)`**：先进先出，`maxsize`为队列大小，其值为非正数时为无限循环队列；
- **`Queue.LifoQueue(maxsize)`**：后进先出，相当于栈；
- **`Queue.PriorityQueue(maxsize)`**：优先级队列。

- 生产-消费者模式实现`demo`：

```python
#!/usr/bin/env python
# coding:utf-8
​
​
import Queue
import threading
import random
​
​
WRITE_LOCK = threading.Lock()
​
​
class Producer(threading.Thread):
    """
    生产-消费者模式（生产者）
    """
​
    def __init__(self, queue, condition, name):
        """
​
        :param queue:
        :param condition:
        :param name:
        """
​
        super(Producer, self).__init__()
        self.queue = queue
        self.name = name
        self.condition = condition
        print "Producer {0} started.".format(self.name)
​
    def run(self):
        """
​
        :return:
        """
​
        while True:
            global WRITE_LOCK
            self.condition.acquire()  # 获取锁对象
            if self.queue.full():
                with WRITE_LOCK:
                    print 'Queue is full, producer wait!'
                self.condition.wait()
            else:
                value = random.randint(0, 10)
                with WRITE_LOCK:
                    print "{name} put value: {value} into queue."\
                        .format(name=self.name, value=value)
                    self.queue.put("{0}: {1}".format(self.name, value))
                    self.condition.notify()
            self.condition.release()
​
​
class Consumer(threading.Thread):
    """
    生产-消费者模式（消费者）
    """
​
    def __init__(self, queue, condition, name):
        """
​
        :param queue:
        :param condition:
        :param name:
        """
​
        super(Consumer, self).__init__()
        self.queue = queue
        self.name = name
        self.condition = condition
        print "Consumer {0} started.".format(self.name)
​
    def run(self):
        """
​
        :return:
        """
​
        while True:
            global WRITE_LOCK
            self.condition.acquire()  # 获取锁对象
            if self.queue.empty():
                with WRITE_LOCK:
                    print 'Queue is empty, consumer wait!'
                self.condition.wait()
            else:
                value = self.queue.get()
                with WRITE_LOCK:
                    print "{name} get value: {value} from queue."\
                        .format(name=self.name, value=value)
                    self.condition.notify()
            self.condition.release()
​
​
if __name__ == '__main__':
​
    qe = Queue.Queue(10)
    con = threading.Condition()
    producer_1 = Producer(qe, con, "P1")
    producer_1.start()
    # producer_2 = Producer(qe, con, "P2")
    # producer_2.start()
​
    consumer_1 = Consumer(qe, con, "C1")
    consumer_1.start()
```