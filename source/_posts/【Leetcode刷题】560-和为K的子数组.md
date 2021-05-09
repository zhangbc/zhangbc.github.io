---
title: 【Leetcode刷题】560. 和为K的子数组
type: categories
copyright: false
date: 2020-05-15 11:34:47
tags: [leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-560
mathjax: true
---


> https://leetcode-cn.com/problems/subarray-sum-equals-k/

# 题目描述

> 给定一个整数数组和一个整数 $k$，你需要找到该数组中和为 $k$ 的连续的子数组的个数。
>

# 思路分析

> 方法一：此题最容易想到的暴力解法即枚举法，从数组的第一个元素开始，累加求和 `sum` 直到数组的最后一个元素结束（数组是无序，需要求的是连续的子数组，千万不能满足找到了第一个子数组就跳出循环，这是很容易忽略的地方），用一个整型变量`counts` 记录 `sum == k` 的个数，然后对第 `2～n` 做同样的处理，最后把每次循环后得到的 `counts` 累加便是本题的答案。注意此方法解题需要注意的几个特例（需要处理的细节问题）：
>
> > 1）子数组可能只含一个元素，如 `[1,1,3] 3`；
> >
> > 2）子数组可能就是整个数组，如 `[28,54,7,-70,22,65,-6] 100`；
> >
> > 3）以某个元素开始的满足条件的子数组可能不止一个，如 `[0,0,0,0,0,0,0,0,0,0] 0`。
>
> 方法二：前缀和+哈希优化法，此方法最不容易想到。对方法一进行优化分析，定义一个新的整型数组 `pre` ，用 `pre[i]` 记录数组的前 `i` 项和，容易得到递推公式：
> $$
> pre[i] = pre[i-1]+nums[i]
> $$
> 那么在 `[j ... i]` 中，和为 `k` 为的条件可表示为：
> $$
> pre[i]-pre[j-1]==k
> $$
> 所以考虑以 `i` 结尾的和为 `k` 的连续子数组个数时只要统计有多少个前缀和为 `pre[i]-k` 的 `pre[j]` 即可。为了简化 `pre` 操作可以用 `hashMap` 存储，定义为 `map`，`key` 为和值 `pre[i]`，`value` 为对应 `pre[i]` 出现的次数，从左往右边更新 `map` 边计算答案，那么以 `i` 结尾的答案 `map[pre[i]−k]` 即可在 $O(1)$ 时间内得到，最终答案即为所有下标结尾的和为 `k` 的子数组个数之和。
>

# 参考代码

- 方法一：暴力枚举法，时间复杂度为 $O(n^2)$，空间复杂度为 $O(1)$。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        if (nums == null || nums.length < 1) {
            return count;
        }

        for (int i = 0; i < nums.length; i++) {
            count += getSums(nums, i, k);
        }
        
        return count;
    }

    private int getSums(int[] nums, int i, int k) {
        int counts = 0;
        int sum = nums[i];
        if (sum == k) {
            counts++;
        }

        while (i < nums.length - 1) {
            sum += nums[++i];
            if (sum == k) {
                counts++;
            }
        }

        return counts;
    }
}
```

运行结果如下：

![暴力枚举法](/images/leetcode_20200515012859.png)

- 方法二：前缀和+哈希优化，时间复杂度为 $O(n)$，空间复杂度为 $O(n)$。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        if (nums == null || nums.length < 1) {
            return count;
        }

        HashMap<Integer, Integer> map = new HashMap<>(16);
        map.put(0,1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (map.containsKey(pre - k)) {
                count += map.get(pre - k);
            }

            map.put(pre, map.getOrDefault(pre, 0) + 1);
        }

        return count;
    }
}
```

运行结果如下：

![前缀和哈希优化](/images/leetcode_20200515102934.png)
