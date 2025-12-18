← [Ashe wiki](../README.md)

---
说一下Golang的内存逃逸

为什么需要逃逸？
Go 中：
栈（Stack）：函数调用时分配，函数返回时自动释放，速度快；
堆（Heap）：手动/垃圾回收管理，生命周期长，但分配/回收开销大。
⚠️ 如果一个函数内的变量在函数返回后仍可能被访问（如通过指针返回），就不能放在栈上（否则会变成“野指针”）。
因此，编译器必须判断：这个变量能否安全地留在栈上？

```go
// example.go
package main

func foo() *int {
    x := 42
    return &x
}

func main() {
    _ = foo()
}
```
go build -gcflags="-m -l" example.go // 查看逃逸分析

