---
title: Go基础语法
tags: Go
---

# 变量
Go语言是一门强类型语言，其中字符串是内置类型，可以直接使用加号拼接。
声明变量的几种方式：
```Go
var name string = ""    //string 可以省略，一般会自动推导变量的类型。
f := 5
```
# 常量
把`var`改为`const`，常量没有确定的类型，需要根据上下文推导。
# if else
if后面没有括号，并且后面必须接大括号。使用分号分隔多个条件。
```Go
if 7 % 2 == 0 {
    fmt.Println("7 is even")
} else {
    fmt.Println("7 is odd")
}
```
# 循环
Go中只有for循环。for循环后面什么都不写即为死循环，可以使用break跳出。
```Go
for {       //死循环
    fmt.Println("loop")
    break
}
```
```Go
for j := 7; j < 9; j++ {
    fmt.Println(j)
}
```
# switch
Go中switch后面不需要括号，case里不加break，并且可以使用`任意`变量类型
```Go
switch a {
case 1:
    fmt.Println("one")
case 2:
    fmt.Println("two")
default:
    fmt.Println("other")
}
```
# 数组
数组变量的定义：
1. var a [5]int
2. b := [5]int{1, 2, 3, 4, 5}
可以直接打印整个数组
```Go
fmt.Println(b)
```
# 切片
1. 使用make创建一个切片
```Go
s := make([]string, 3)
```
2. 使用append添加多个元素，注意有返回值且返回值要赋值给s
```Go
s = append(s, "d", "f")
```
因为当容量不够时会扩容并返回新的切片。
# map
**创建方法**：使用make创建一个map，需要key和value的类型
```Go
m := make(map[string]int)
```
**注意**：map是完全无序的，按照随机顺序遍历。
# range
使用range能快速遍历，它会返回两个值：索引、对应位置的值。
```
package main
import "fmt"
func main () {
    nums := []int{2, 3, 4}
    sum := 0
    for i, num := range nums {
        sum += num
        if num == 2 {
            fmt.Println("index:", i, "nums:", num)
        }
    }
    fmt.Println(sum)

    m := map[string]string{"a":"A", "b":"B"}
    for k, v := range m {
        fmt.Println(k, v)
    }
    for k := range m {
        fmt.Println("key", k)
    }
}
```
# 函数
函数的返回类型是放在后面的。例如：
```Go
func exists(m map[string]string, k string) (v string, ok bool) {
    v, ok = m[k]
    return v, ok
}
```
返回的两个值v和ok, 他们的类型分别是是string和bool
# 指针
指针的操作有限，主要是对传入的参数进行修改
```Go
func add(n *int) {
    *n += 2
}
```
# 结构体
定义一个结构体：`type xxx struct {}`，例如
```
type user struct {
    name string
    password string
}
```
定义结构体变量：
```Go
a := user{name: "wang", password: "1024"}
b := user{"wang", "1024"}
```
定义结构体方法：
```Go
func (u *user) resetPassword(password string) {
    u.password = password
}
```
在func后面加`结构体的名字和形参`，就变成了该结构体的方法，可以使用结构体变量调用这个方法。
# 错误处理
在Go中使用一个单独的返回值来传递错误信息
```Go
func findUser(users []user, name string) (v *user, err error) {
    for _, u := range users {
        if u.name == name {
            return &u, nil
        }
    }
    return nil, errors.New("not found")
}
```
Go能很清晰地知道哪一个函数返回了错误，并能用if-else处理错误。
```
if u, err := findUser([]user{{"wang", "1024"}}, "li"); err != nil {
    fmt.Println(err)
    return 
} else {
    fmt.Println(u.name)
}
```
# JSON处理
需要`import "encoding/json"`
对于一个结构体，要保证每个字段的第一个字母是大写（即`公开字段`），这个结构体就能用`JSON.marshaler`来序列化。
```Go
type userInfo struct {
    Name string
    Age int `json:"age"`    //使用json tag语法来修改输出JSON结果里面的字段名
    Hobby []string
}
a := userInfo{Name: "wang", Age: 18, Hobby: []string{"Golang", "TypeScript"}}
buf, err = json.MarshalIndex(a, "", "\t")
if err != nil {
    panic(err)
}
fmt.Println(string(buf))
```
# 时间处理
需要`import "time"`
获取当前时间：`now := time.Now()`
获取时间戳：`fmt.Println(now.Unix())`
# 数字解析
需要`import "strconv"`
解析整数：
```Go
n, _ := strconv.ParseInt("111", 10, 64) //10进制，精度是64位
```
# 进程信息
需要`import "os" "os/exec"`
获取程序执行时指定的命令行参数：`fmt.Println(os.Args)`
读取环境变量：`fmt.Println(os.Getenv("PATH")`

