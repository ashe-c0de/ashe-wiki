← [Ashe wiki](../README.md)

---
context（上下文）是 控制并发操作生命周期、传递请求范围数据 的核心机制。

1. 取消操作（Cancellation）

2. 设置超时（Timeout / Deadline）

3. 传递请求作用域的值（Request-scoped values）

context包如何实现goroutine的取消?
每次调用 WithCancel(parent)，都会创建一个新的 cancelCtx，并注册到父 context 的 children 中；取消goroutine的时候，通过channel通道传递一个空struct，子context通过监听channel，
