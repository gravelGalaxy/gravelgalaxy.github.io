---
title: Go进阶与依赖管理
tags: Go
---

# Go语言进阶
## 协程
Go可以充分发挥多核优势，高效运行。

协程和线程：
协程属于用户态，可以理解成轻量级线程，栈KB级别。
线程属于内核态，线程跑多个协程，栈MB级别。

1. 快速打印hello goroutine 0 ~ 4
```Go
func hello (i int) {
    println("hello goroutine : " + fmt.Sprint(i))
}

func HelloGoRoutine() {
    for i := 0; i < 5; ;i++ {
        go func(j int) {
            hello(j)
        }(i)
    }
    time.Sleep(time.Second)
}
```
channel 通道:提倡通过通信共享内存而不是通过共享内存而实现通信。

## channel
make(chan 元素类型，[缓冲大小])
* 无缓冲通道(同步通道)`make(chan int)`
* 有缓冲通道`make(chan int, 2)`

### 示例

要求：
* 子协程生产数字
* 另一个接受数字并计算平方数

Q:
* 为什么是并发安全的？
* 带缓冲的chan解决生产和消费速度差异带来的问题
```Go
func CalSquare() {
    src := make(chan int)
    dest := make(chan int, 3)
    go func() {
        defer close(src)
        for 

```

## 并发安全Lock
```Go
var (
    x int 64
    lock sync.Mutex
)

func addWithLock() {
    for i := 0; i < 2000; i++ {
        lock.Lock()
        x += 1
        lock.Unlock()
    }
}

```

## WaitGroup
计数器：
开启协程+1, 执行结束-1；主协程阻塞知道计数器为0

```Go
func ManyGoWait() {
    var wg sync.WaitGroup
    wq.Add(5)
    for i := 0; i < 5; i++ {

```


# 依赖管理
依赖管理演进：
GOPATH-->Go Vendor --> GO MODULE
1. GOPATH:环境变量，项目代码直接依赖src下的代码
* bin
* pkg:项目编译的中间产物, 加速编译
* src
缺陷场景：A和B依赖于某一package的不同版本，无法实现package的多版本控制。
2. Go Vendor
* 项目目录增加vendor文件，保存依赖包副本。解决了多个项目需要同一个package依赖的冲突问题。
缺陷场景：A依赖B和C,而B和C又依赖D的不同版本。更新项目时可能出现依赖冲突，导致编译失败。
3. Go mod：go.mod文件管理依赖包版本
通过go get/go mod指令工具管理依赖包。
* 配置文件：go.mod
* 中心仓库管理依赖库：Proxy
* 本地工具：go get/mod

```
module example/project/app  //依赖管理基本单元

go 1.16 //原生库

require (   //单元 依赖
    example/lib1 v1.0.2
    example/lib2 ...  //indirect非直接依赖
    example/lib3
    example/lib4
    example/lib5 ... incompatible主版本2+模块会在模块路径增加/vN后缀,可能有不兼容的代码逻辑
)
```
1. 语义化版本
${MAJOR}.${MINOR}.${PATCH}
MAJOR间相互隔离
MINOR依赖于MAJOR
PATCH修改bug
2. 基于commit伪版本

## 依赖分发-回源
使用Proxy得到稳定、可靠的分发。
GOPROXY="https://proxy1.cn, https://proxy2.cn, direct"
类似于缓存：Proxy1 -> Proxy2 -> direct
direct表示源站

## go get go mod
go get example.org/pkg

go mod 
```
init 初始化，创建go.mod
download 下载模块到本地缓存
tidy    增加需要的依赖，删除不需要的依赖
```


