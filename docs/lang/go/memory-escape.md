Golang的内存逃逸

为什么需要逃逸？
Go 中：
- 栈（Stack）：函数调用时分配，函数返回时自动释放，速度快；
- 堆（Heap）：手动/垃圾回收管理，生命周期长，但分配/回收开销大。  

⚠️ 如果一个函数内的变量在函数返回后仍可能被访问（如通过指针返回），就不能放在栈上（否则会变成“野指针”，因为函数结束内存释放了，这个指针指向的数据就是空或者错误的数据）。
因此，编译器必须判断：这个变量能否安全地留在栈上？

```go
package main

import "fmt"

func main() {
	do := Do()
	fmt.Println(do)
}

func Do() *int {
	fmt.Println("Do begin")
	res := 7
	fmt.Println("Do end")
	return &res
}
```
go build -gcflags="-m" -o NUL ./sth/main.go // 查看逃逸分析

```bash
PS C:\workspace\Golang_project\interview> go build -gcflags="-m" -o NUL ./sth/main.go
# command-line-arguments
sth/main.go:11:13: inlining call to fmt.Println
sth/main.go:13:13: inlining call to fmt.Println
sth/main.go:7:13: inlining call to fmt.Println
sth/main.go:12:2: moved to heap: res
sth/main.go:11:13: ... argument does not escape
sth/main.go:11:14: "Do begin" escapes to heap
sth/main.go:13:13: ... argument does not escape
sth/main.go:13:14: "Do end" escapes to heap
sth/main.go:7:13: ... argument does not escape
```