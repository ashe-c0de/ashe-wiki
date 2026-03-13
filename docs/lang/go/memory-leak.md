Golang的内存泄漏

- Channel 阻塞导致的 Goroutine 泄漏
- 对象一直被引用，GC 无法回收
- 切片（slice）扩容导致底层数组无法释放
- 未关闭资源（间接导致内存泄漏）


> 简而言之，Go 的内存泄漏 = “对象因被意外引用而无法被 GC 回收”

当怀疑有内存泄漏时，可以使用 Go 的工具链—— pprof (Heap Profile)：  

[模拟内存泄漏的web应用](https://github.com/ashe-c0de/lang-lab/blob/main/golang/trace/pprof.go)

`go tool pprof http://localhost:6060/debug/pprof/heap`
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/go/go-pprof.png)

`top`
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/go/go-pprof-top.png)

`list  main.main.func3`
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/go/go-pprof-list.png)