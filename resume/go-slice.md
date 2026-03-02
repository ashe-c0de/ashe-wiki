← [Ashe wiki](../README.md)

---
Go 的 slice 底层包含三个重要组成部分：

```go
type slice struct {
	array unsafe.Pointer // 指向底层数组中第一个元素的地址
	len   int // 长度，切片当前包含的元素个数。
	cap   int // 容量，切片从起始位置到底层数组末尾所能容纳的最大元素个数
}

```

当 append 导致 len > cap 时: (触发扩容)

Go 会分配一个新的底层数组，并将原数据复制过去。扩容策略大致如下（具体实现可能随版本优化）：  
如果原容量 < 1024，新容量 ≈ 原容量 * 2  
如果原容量 ≥ 1024，新容量 ≈ 原容量 * 1.25

```go
package main

import "fmt"

func main() {

	// 定义len和cap都为5的slice
	s0 := make([]int, 5)

	for i := range s0 {
		fmt.Println(s0[i]) // 输出5个0
	}

	// 定义len为3，cap为5的slice
	s := make([]int, 3, 5)
	// 长度为3，可用元素index只有0、1、2
	fmt.Println(s[3]) // panic: runtime error: index out of range [3] with length 3

	// 推荐使用这种方式初始化slice，因为append默认在index(len)处赋值
	s1 := make([]int, 0, 5)

	s2 := make([]int, 2)
	s2 = append(s2, 9)
	fmt.Println(s2) // 输出 0 0 9
	fmt.Println(len(s2)) // 3
	fmt.Println(cap(s2)) // 4
}
```

