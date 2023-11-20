---
title: 高精度算法
tags: Algorithm
---

# 高精度加法
## 模板
```C++
//A和B要逆序将输入的整数保存。即数组从数值个位开始存。
//输出的结果C也是逆序的。
vector<int> add(vector<int> &A, vector<int> &B) { 
    vector<int> C;
    int t;
    for (int i = 0; i < A.size() || i < B.size(); i++) {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(t);
    return C;
}
```
## 要点
1. 要将数`逆序`保存在数组中，先算个位。
2. 使用`vector`自动扩充。
3. `t % 10`为当前位的值，`t / 10` 为进位。
