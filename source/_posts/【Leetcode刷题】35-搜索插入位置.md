---
title: 【Leetcode刷题】35. 搜索插入位置
type: categories
copyright: false
date: 2020-06-01 22:09:24
tags: [leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-lcof-35
mathjax: true
---


> 原题链接：https://leetcode-cn.com/problems/search-insert-position/

# 题目描述

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 你可以假设数组中无重复元素。

# 思路分析

> 分析题意，可以理解为给一个已排好序的数组“插入”一个数，求解其插入位置。这里给出两种解法。
>
> 方法一：常规解法，逐一遍历每一个元素并与目标值比较，如果目标值大于前一个数而小于等与后一个数，那么应返回后一个数的位置，即为插入位置；如果目标值小于第一个元素应返回0；如果目标值大于最后一个元素应返回数组的长度，即为插入位置；
>
> 方法二：二分查找法，进一步探究方法一，其实质就是查找，那么可以优化查找算法，从而可以应用二分法查找，降低时间复杂度。

# 参考代码

- 方法一：常规解法，其时间复杂度为 $O(n)$，空间复杂度为 $O(1)$。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        if (nums == null || nums.length < 1) {
            return 0;
        }

        for (int i = 0; i < nums.length; i++) {
            if (target <= nums[i]) {
                return i;
            }
        }

        return nums.length;
    }
}
```

运行结果如下：

![常规解法](/images/leetcode_20200601215744.png)

- 方法二：二分查找法，其时间复杂度为 $O(log{n})$，空间复杂度为 $O(1)$。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        if (nums == null || nums.length < 1) {
            return 0;
        }

        int low = 0, high = nums.length - 1, mid;
        while (low <= high) {
            mid = (low + high) / 2;
            if (target < nums[mid]) {
                high = mid - 1;
            } else if (target == nums[mid]) {
                return mid;
            } else {
                low = mid + 1;
            }
        }
        
        return low;
    }
}
```

运行结果如下：

![二分查找法](/images/leetcode_20200601215004.png)

