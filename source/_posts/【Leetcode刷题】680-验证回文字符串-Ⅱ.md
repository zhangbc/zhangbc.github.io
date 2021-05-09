---
title: 【Leetcode刷题】680. 验证回文字符串 Ⅱ
type: categories
copyright: false
date: 2020-05-19 15:02:36
tags: [leetcode, 数据结构与算法]
categories: Java
title_en: leetcode-680
mathjax: true
---


> 原题链接：https://leetcode-cn.com/problems/valid-palindrome-ii/

# 题目描述

> 给定一个非空字符串 `s`，**最多**删除一个字符。判断是否能成为回文字符串。
>
> **注意：** 字符串只包含从 `a-z` 的小写字母。字符串的最大长度是50000。

# 思路分析

> 这题是回文判断的一个变式题，主要还是考查对双指针的理解。用 `low` 和 `high` 分别指向字符串 `s` 的首位和末尾，如果二者相等，则执行 `low++` ，`high--` ；如果不相等，则需要分两种情况：
>
> > 1）第 `low + 1` 位与第 `high` 位相等，依次循环判断，如果为回文字符串，则返回 `true` ，否则返回 `false` ，记为 `usedLeft`；
> >
> > 2）第 `low` 位与第 `high - 1` 位相等，依次循环判断，如果为回文字符串，则返回 `true` ，否则返回 `false` ，记为 `usedRight`。
>
> 最后，综合不等情况判断，只要不等情况1）和2）中有一个符合回文字符串，那么都应该返回 `true`，即判断条件应为  `usedLeft||usedRight`  ；跳出循环如果还没返回值，说明是标准的回文串，应返回 `true`。
>
> 做题过程中，对不等判断很容易忽视情况1）和2）是**或**的关系判断， 即忽略判断两边都可以的情况（**贪心算法思想**）。如测试实例：
> `"aguokepatgbnvfqmgmlcupuufxoohdfpgjdmysgvhmvffcnqxjjxqncffvmhvgsymdjgpfdhooxfuupuculmgmqfvnbgtapekouga"`。

# 参考代码

- 双指针法，时间复杂度为 $O(n)$，空间复杂度为 $O(1)$。

```java
class Solution {
    public boolean validPalindrome(String s) {
        if (s == null || s.length() < 1) {
            return false;
        }

        int low = 0, high = s.length() - 1;
        while (low < high) {
            if (s.charAt(low) == s.charAt(high)) {
                low++;
                high--;
                continue;
            }

            int left = low + 1, right = high - 1;
            boolean usedLeft = isPalindrome(s, left, high);
            boolean usedRight = isPalindrome(s, low, right);
            return usedLeft || usedRight;
        }

        return true;
    }

    private boolean isPalindrome(String s, int low, int high) {
        while (low < high) {
            if (s.charAt(low) != s.charAt(high)) {
                return false;
            }
            
            low++;
            high--;
        }
        
        return true;
    }
}
```

运行结果如下：

![双指针法](/images/leetcode_20200519104743.png)

