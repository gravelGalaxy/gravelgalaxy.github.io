---
title: constexpr
date: 2024-04-01 10:58:43
tags: C++
---

# constexpr 修饰常量表达式
## constexpr可以修饰static变量。
```
static constexpr QLatin1StringView name = QLatin1StringView("qtFileMenu");
```
这样，name的值在编译时确定，并且保存到静态存储区。
## constexpr修饰函数时，函数会根据参数是不是常量表达式来判断函数是不是常量表达式。
```
constexpr int test(int cnt) {
    return 3 * cnt;
}

...
test(2); // 是常量表达式。
int i = 3;
test(i); // 不是常量表达式，不会在编译时确定。
constexpr int j = 4;
test(j); // 常量表达式
```

