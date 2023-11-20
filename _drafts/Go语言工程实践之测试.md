---
title: Go语言工程实践之测试
tags: Golang
---

# 回归测试

# 集成测试

# 单元测试

输入->测试单元->输出
-------期望-------->校对

## 规则

1. 所有测试文件以 _test.go 结尾
2. fucn TestXxx(*testing.T)
3. 初始化逻辑放到TestMain中
```
m *testing.T
code := m.Run()
```

```Go
func TestHelloTom(t *testing.T) {
    output := HelloTom()
    expectOutput := "Tom"
    if output != expectOutput {
        t.Error("Expected %s do not match actual %s", expectOutput, output)
    }
}
```
assert.Equal(t, expectOutput, output)

覆盖率：如何衡量代码是否经过了足够的测试？
如何评价项目的测试水准？

一般覆盖率：50%～60%， 较高覆盖率80%
测试分支项目独立，全面覆盖
测试单元粒度足够小，函数单一职责

外部依赖：稳定和幂等（每次结果一样）

快速Mock函数：有依赖时用
* 为一个函数打桩(用一个函数A:`打桩函数`替换B)
* 为一个方法打桩

```
func Patch(target, replacement interface{}) *PatchGuard {
}

func UnPatch(
```
不再依赖本地文件
# 基准测试
对代码进行分析
```
以Benchmark开头
有性能问题：
func BenchmarkSelect(b *testing.B) {
    InitServerIndex()
    b.ResetTimer()
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next

```


