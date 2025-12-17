← [Ashe wiki](../README.md)

---
Go 的 slice 底层包含三个重要组成部分：

- 指向底层数组中 slice 第一个元素的地址（Data）
- 当前长度（Len）
- 容量（Cap）

当 append 导致 len > cap 时，Go 会分配一个新的底层数组，并将原数据复制过去。扩容策略大致如下（具体实现可能随版本优化）：  
如果原容量 < 1024，新容量 ≈ 原容量 * 2  
如果原容量 ≥ 1024，新容量 ≈ 原容量 * 1.25

```go
package main

import "fmt"

func main() {
    s := make([]int, 3, 5)
    fmt.Println(s[3]) // panic: runtime error: index out of range [3] with length 3
}
```
定义如上所示的一个slice，你会觉得len和cap完全没有区别，但其实当你尝试获取s[3]的时候就发生了错误，因为len代表slice的可用元素个数，cap代表起始指针位置到整个底层数组末尾的剩余元素个数。
