---
title: 优先队列的自定义比较器
tags: C++
date: 2023-04-27 22:19:18
---

# 什么是自定义比较器
> A binary predicate that takes two elements (of type T) as arguments and returns a bool.
The expression comp(a, b), where *comp* is an object of this type and *a* and *b* are elements in the container, shall return **true** if *a* is considered to go before *b* in the *strict weak ordering* the function defines.
> The `priority_queue` uses this function to maintain the elements sorted in a way that preserves heap properties(i.e., that the element popped is the last according to this *strict weak ordering*).
This can be a function pointer or a function object, and defaults to `less<T>`, which returns the same as applying the *less-than* operator(`a < b`).
## 名词解释
`container`即容器，像`queue`、`vector`、`list`都表示一个容器。
`strict weak ordering`(严格弱序)，即重载的运算符需要满足的三个条件：
1. **strict**: 对于表达式`s < s`，必须返回`false`。
2. **weak**: 如果表达式`s < t`和表达式`t < s`都返回`false`，则`s == t`。
3. **ordering**: 如果有`s < t`且`t < z`，则有`s < z`。
## 要点
1. 要接受两个参数，返回一个bool值。
2. comp要是一个函数对象，也可以是一个函数指针。定义函数对象/比较器是本文的主要内容。
3. 当比较器返回`true`时，`a`和`b`的位置不变；反之`a`和`b`交换位置。像优先队列中如果比较器是`less<T>`，当插入一个元素时该元素首先被放在堆的末尾，紧接着该元素作为`a`，其父节点作为`b`，如果`a < b`那么新的节点/元素与父节点之间不交换位置，如此可以构建一个大顶堆。

# 自定义比较器的方法
通过不同方法定义的比较器，在使用时也会有**差异**。
## 方法一：通过struct重载运算符`()`
```C++
//自定义比较器
struct cmp {
    bool operator() (pair<char, int> &a, pair<char, int> &b) {
        return a.first > b.first;
    }
};
//在优先队列中使用该比较器
priority_queue<pair<char, int>, vector<pair<char, int>>, cmp> pq;
```
## 方法二：通过class重载运算符`()`
```C++
class cmp {
public:
    bool operator() (pair<char, int> &a, pair<char, int> &b) {
        return a.first > b.first;
    }
};
priority_queue<pair<char, int>, vector<pair<char, int>>, cmp> pq;
```
## 方法三：通过定义函数并转为函数指针的方式来自定义比较器
```C++
bool cmp (pair<char, int> &a, pair<char, int> &b) {
    return a.first > b.first;
}
priority_queue<pair<char, int>, vector<pair<char, int>>, decltype(&cmp)> pq(cmp);
```
## 方法四：使用Lambda表达式
```C++
auto cmp = [](pair<char, int> &a, pair<char, int> &b) -> bool {
    return a.first > b.first;
}
priority_queue<pair<char, int>, vector<pair<char, int>>, decltype(cmp)> pq(cmp);
```
## 方法五：通过funcion包装Lambda表达式
需要头文件`#include <functional>`
```C++
function <bool (pair<char, int>&, pair<char, int>&)> cmp = [] (pair<char, int> &a, pair<char, int> &b) {
    return a.first > b.first;
}
priority_queue<pair<char, int>, vector<pair<char, int>>, decltype(cmp)> pq(cmp);
```



