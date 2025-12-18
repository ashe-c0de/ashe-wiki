← [Ashe wiki](../README.md)

---
说一下Golang的内存泄漏

- channel 发送/接受阻塞，导致其所在的goroutine永久阻塞（goroutine泄漏，即父协程关闭了，子协程仍然由于阻塞无法正常退出）
- 全局变量或长生命周期对象持有引用
- 切片（slice）扩容导致底层数组无法释放
- 未关闭资源（间接导致内存泄漏）

```
// 将短生命周期对象存入全局 map、slice 或结构体字段中，即使业务逻辑已完成，这些对象仍被强引用。
var cache = make(map[string]*UserData)

func handler(id string) {
    user := loadUser(id)
    cache[id] = user // 若不清理，user 永远不会被 GC
}
```

> 简而言之，Go 的内存泄漏 = “对象因被意外引用而无法被 GC 回收”
