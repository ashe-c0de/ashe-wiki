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

	// 定义len和cap都为5的slice
	s0 := make([]int, 5)

	for i := range s0 {
		fmt.Println(s0[i]) // 输出5个0
	}

	// 定义len为3，cap为5的slice
	s := make([]int, 3, 5)
	// 长度为3，可用元素index只有0、1、2
	fmt.Println(s[3]) // panic: runtime error: index out of range [3] with length 3
}
```

