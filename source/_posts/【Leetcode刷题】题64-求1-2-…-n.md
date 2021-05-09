---
title: 【Leetcode刷题】题64. 求1+2+…+n
type: categories
copyright: false
date: 2020-06-02 13:42:35
tags: [剑指offer, leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-lcof-03
mathjax: true
---


> 原题链接：https://leetcode-cn.com/problems/qiu-12n-lcof/

# 题目描述

> 求 `1+2+...+n` ，要求不能使用乘除法、`for`、`while`、`if`、`else`、`switch`、`case` 等关键字及条件判断语句（`A?B:C`）。

# 思路分析

> 本题抛开要求限制，算是入门级题目，但是加上条件限制却变成了一道思维拓展题，不涉及任何算法知识。这里给出两种解决方案：
>
> 方法一：递归法，递归需要判断终止条件，除了 `if` 语句，还有 `switch`，逻辑运算符，进而可以突破 `if` 的限制，达到解题目的；
>
> 方法二：等差数列求和公式，$$S_{n}=\frac{(1+n)*n}{2}$$，这里出现了乘除法，现在就需要想方设法找道乘除法的替代方案，除法可以用位运算替代；乘法呢？将分子展开即有 $$n^2 + n$$，平方可以调用库函数 `pow`。

# 参考代码

- 方法一：递归法，其时间复杂度为 $O(n)$ ，空间复杂度为 $O(1)$ 。

```java
class Solution {
    public int sumNums(int n) {
        int sum = n;
        boolean flag = n > 0 && (sum += sumNums(n - 1)) > 0;
        return sum;
    }
}
```

运行结果如下：

![递归法](/images/leetcode_20200602003011.png)

- 方法二：等差数列公式，其时间复杂度为 $O(1)$ ，空间复杂度为 $O(1)$ 。

```java
class Solution {
    public int sumNums(int n) {
        return ((int)Math.pow(n, 2) + n) >> 1;
    }
}
```

运行结果如下：

![等差数列公式](/images/leetcode_20200602113841.png)