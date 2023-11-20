---
title: 二分检索
tags: Algorithm
date: 2023-05-12 19:17:15
---

# 二分检索介绍
二分检索的核心`不在于`数列是递增的，而在于数列可以按照某个条件按`真假`值分两个部分。在每一次循环或递归，解的范围都能够缩小一半。

# 整数二分检索模板
```C++
int binary_search(int q[], int target, int l, int r) {
    if (l >= r) return l;
    int mid = l + r + 1 >> 1;
    if (left_check(mid)) l = mid;
    else r = mid - 1;
    return binary_search(q, target, l, r);
}
```
要点：当满足左半边的条件时，说明要查找的元素在`mid加上右半边`，此时应该将`l`更新为`mid`。为了不出现死循环，注意`l`和`r`的中间节点`mid`应该`向上取整`，来保证`l`的值会发生`更新`；而`mid <= r`，若不满足`左半边条件`，`r`的值一定会发生更新，不会出现死循环。
```C++
int binary_search(int q[], int target, int l, int r) {
    if (l >= r) return l;
    int mid = l + r >> 1;
    if (right_check(mid)) r = mid;
    else l = mid + 1;
    return binary_search(q, target, l, r);
}
```
要点：相似的，当满足右半边的条件时，说明要查找的元素在`mid加上左半边`，此时应该将`r`更新为`mid`。此时`mid`应该`向下取整`，来保证`r`的值会发生`更新`；而`mid >= l`，若不满足`右半边条件`，`l`的值一定会发生更新，不会出现死循环。
# 浮点数二分检索模板（开方）
```C++
int binary_search(double target, double l, double r) {
    double mid = (l + r) / 2;           //不能右移
    while (r - l < 1e-8) {
        if (mid * mid > target) r = mid;
        else l = mid;
    }
    return r;
}
```
