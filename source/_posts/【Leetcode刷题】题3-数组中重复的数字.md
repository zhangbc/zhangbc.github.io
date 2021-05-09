---
title: 【Leetcode刷题】题3.数组中重复的数字
type: categories
copyright: false
date: 2020-05-08 23:12:08
tags: [剑指offer, leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-lcof-03
mathjax: true
---


> 原题链接： https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof

# 题目描述

> 在一个长度为 $n$ 的数组 $nums$ 里的所有数字都在 $0$ ~ $n-1$ 的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复，也不知道每个数字重复几次。请找出数组中任意一个重复的数字。

# 思路分析

> 通过阅读提干，不难发现，题目要求是从已知数组中找到重复元素即可。这里列出三种解题思路仅作参考：
>
> > 1）最容易想到的算法自然是先排序后通过依次遍历并与后一个元素相比较找出第一个重复元素即可，易想到但是算法代价大不可取，最优时间复杂度为 $O(nlogn)$ ；
> >
> > 2）利用数据结构--`hash`结构，依次遍历数组中每一个元素，同时将元素放入`hash`中，在放入前先做判断：如果在`hash`存在则说明找到重复元素，否则放入`hash`中，直到遍历完所有元素为止，其时间复杂度为 $O(n)$ ，空间复杂度为 $O(n)$ ；
> >
> > 3）充分利用已知条件“数组里的所有数字都在 $0$ ~ $n-1$ 的范围内”，通过交换元素位置比较可以发现是否有重复元素，为本题最优解，其时间复杂度为 $O(n)$ ：
> >
> > > （1）原地改动数组，空间复杂度为 $O(1)$ ；
> > >
> > > （2）定义新数组，空间复杂度为 $O(n)$ 。


# 参考代码

- 方法一：复制数组，时间复杂度为 $O(n)$ ，空间复杂度为 $O(n)$ 。

````java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] arr = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            arr[nums[i]]++;
            if (arr[nums[i]] > 1 ) {
                return nums[i];
            }
        }

        return -1;
    }
}
````

运行结果如下：

![复制数组](/images/leetcode_20200508234733.png)

- 方法二：原地修改数组，时间复杂度为 $O(n)$ ，空间复杂度为 $O(1)$ 。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i) {
                if (nums[nums[i]] == nums[i]) {
                    return nums[i];
                }
                
                int temp = nums[nums[i]];
                nums[nums[i]] = nums[i];
                nums[i] = temp;
            }
        }

        return -1;
    }
}
```

运行结果如下：

![原地修改数组](/images/leetcode_20200508234657.png)