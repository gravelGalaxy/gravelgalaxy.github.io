---
title: Go实战案例
tags: Go
---

# 猜谜游戏 - 生成随机数
使用前需要设置随机数种子jjjj
```Go
import "math/rand"

func main () {
maxNum := 100
rand.Seed(time.Now().UnixNano())
secretNumber := rand.Intn(maxNum)
}
```
读取用户输入
```Go
import "os"
reader := bufio.NetReader(os.Stdin)
input, err := reader.ReadString('\n')
input = string.TrimSuffix(input, "\n")
guess, err := strconv.Atoi(input)
```
# 在线词典
首先获取词典
发送http,解析json
fanyi.caiyunapp.com
开发者工具，找到dict的post请求，点击payload和preview
右键请求，选择copy as curl
curlconverter.com/#go
流式创建请求，否则内存占用的
request body和reponse body
oktools.net/json2go
```Go
if resp.StatusCode != 200 {
}
```
# Socks5 代理
四个阶段：握手阶段、认证阶段、请求阶段、relay阶段
1. 握手阶段：`浏览器`向`socks5代理`发送请求，包括协议的`版本号`、支持认证的`种类`。socks5会选择一个`认证方式`，返回给浏览器。返回`00`就不需要认证，否则开始认证流程。
2. 认证阶段
3. 请求阶段：认证通过后`浏览器`会向`socks5`服务器发送请求，包括版本号、请求的类型，一般是`connection`请求，代表`代理服务器`要和某个域名或者`某个IP地址某个端口`建立`TCP`连接。`代理服务器`响应之后，会真正和`后端服务器`建立连接，然后返回一个`响应`。
4. relay阶段：浏览器会发送`正常发送`请求，然后`代理服务器`接收到请求之后，会直接把请求转换到`真正的服务器`上。如果真正的服务器返回响应，那么也会把请求`转发`到浏览器。实际上代理服务器并不关心流量的细节，可以是HTTP流量，也可以是其他TCP流量。
```Go
package main
import (
    "bufio"
    "log"
    "net"
)
func main() {
    server, err := net.Listen("tcp", "127.0.0.1:1080")
    if err != nil {
        panic(err)
    }
    for {
        client, err := server.Accept()      //返回一个连接conn
        if err != nil {
            log.Printf("Accept failed %v", err)
            continue
        }
        go process(client)                  //在goroutine里执行这个连接
    }
}
func process(conn net.Conn) {
    defer conn.Close() 
    reader := bufio.NewReader(conn)         //这个conn可以作为reader的参数
    for {
        b, err := bufio.ReadByte()
        if err != nil {
            break
        }
        _, err = conn.Write([]byte{b})
        if err != nil {
            break
        }
    }
}
```
使用`nc -v 127.0.0.1 1080`命令可以测试是否能成功进行TCP连接。


## TCP echo server
发送什么，返回什么
go process(client) 类似于子线程

nc命令建立TCP连接
## auth函数
## 请求阶段

go routine
context.WithCancel(context.Background())
<-ctx.Done()
## relay 阶段
Switchy Omega插件
新建情景模式


