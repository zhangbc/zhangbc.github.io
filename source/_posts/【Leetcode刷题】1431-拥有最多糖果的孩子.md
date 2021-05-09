---
title: 【Leetcode刷题】1431. 拥有最多糖果的孩子
type: categories
copyright: false
date: 2020-06-01 20:42:59
tags: [leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-lcof-1431
mathjax: true
---



> 原题链接：https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/

# 题目描述

> 给你一个数组 `candies` 和一个整数 `extraCandies` ，其中 `candies[i]` 代表第 `i` 个孩子拥有的糖果数目。
>
> 对每一个孩子，检查是否存在一种方案，将额外的 `extraCandies` 个糖果分配给孩子们之后，此孩子有**最多**的糖果。注意，允许有多个孩子同时拥有**最多**的糖果数目。
>
> 示例 1：
>
> > 输入：`candies = [2,3,5,1,3], extraCandies = 3`
> > 输出：`[true,true,true,false,true]` 
> > **解释**：
> > 孩子 1 有 2 个糖果，如果他得到所有额外的糖果（3个），那么他总共有 5 个糖果，他将成为拥有最多糖果的孩子。
> > 孩子 2 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
> > 孩子 3 有 5 个糖果，他已经是拥有最多糖果的孩子。
> > 孩子 4 有 1 个糖果，即使他得到所有额外的糖果，他也只有 4 个糖果，无法成为拥有糖果最多的孩子。
> > 孩子 5 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
>
> 示例 2：
>
> > 输入：`candies = [4,2,1,1,2], extraCandies = 1`
> > 输出：`[true,false,false,false,false]`
> > **解释**：只有 1 个额外糖果，所以不管额外糖果给谁，只有孩子 1 可以成为拥有糖果最多的孩子。

# 思路分析

> 对题目理解不透彻的话，强烈建议看示例中解释，很好地帮助我们审了题意，即本题要求是：针对组内每一个小朋友手中糖果`candies[i]` ，若把额外的糖果 `extraCandies` 分配给他，判断能否达到或者超过组内糖果的最大值。因而，分两步走：1）求出组内最大值 `max`；2）针对组内每一个元素加上 `extraCandies` 判断能否达到或超过最大值，若能记为 `true`，否则记为 `false`，将结果保存为一个数组并返回。

# 参考代码

- 常规解法，其时间复杂度为 $O(n)$，空间复杂度为 $O(n)$。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        List<Boolean> result = new ArrayList<>();
        if (candies == null || candies.length < 1) {
            return result;
        }

        int max = candies[0];
        for (int value: candies) {
            max = Math.max(value, max);
        }

        for (int value: candies) {
            result.add(value + extraCandies >= max);
        }
        
        return result;
    }
}
```

运行结果如下：

![常规解法](/images/leetcode_20200601100250.png)

各位看官，六一快乐，希望未来有更多的伙伴入坑～