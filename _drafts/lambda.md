---
title: lambda表达式
tags: C++
---

# Lambda的主要作用
Lambda解决了`C++最令人头痛的语法解析问题`。即当C++编译器解析如下语句时：
```C++
class background_tack {
public:
    void operator() () const {
        do_something();
        do_something_else();
    }
}
std::thread my_thread (background_task());
```
C++会将`std::thread my_thread (background_task());`解析为函数声明。为了解决该问题，可以使用Lambda表达式：
```
std::thread my_thread ([] {
    do_something();
    do_something_else();
});
```
这样就实例化了一个`std::thread`类的对象`my_thread`，其参数是Lambda表达式。
