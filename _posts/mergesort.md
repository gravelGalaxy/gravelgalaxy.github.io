---
title: 归并排序
tags: Algothrim
date: 2023-05-12 19:27:30
---


# 归并排序模板
```C++
void merge_sort(int q[], int temp[], int l, int r) {
    if (l >= r) return ;
    int m = l + r >> 1, i = l, j = m + 1, k = 0;
    merge_sort(q, l, m);
    merge_sort(q, m + 1, r);
    while (i <= m && j <= r) 
        if (q[i] < q[j]) temp[k++] = q[i++];
        else temp[k++] = q[j++];
    while (i <= m) temp[k++] = q[i++];
    while (j <= r) temp[k++] = q[j++];
    for (i = l, j = 0; i <= r; i++, j++) q[i] = temp[j];
    return;
}
```
## 步骤
1. 首先递归到`最底层`，即`l : r`限定的范围中只有一个元素。
2. 对数列`q[l : m]`和`q[m + 1 : r]`中的数进行比较，这两个数列内部已经是按照从小到大排列的了。该步骤需要一个临时数组`temp`，长度为`r - l + 1`, 将两个有序数组合并成一个有序数组。
3. 将各数组中`剩下的元素`保存到temp数组中。
4. 将temp数组中的元素`覆盖原数组`。
## 注意
注意边界问题，`l : m` 属于第一个数列，`m + 1 ：r` 属于第二个数列，在`while`循环中都要遍历到边界值(分别是`m` 和 `r`)
