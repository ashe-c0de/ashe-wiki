← [Ashe wiki](../README.md)

---
context（上下文）是 控制并发操作生命周期、传递请求范围数据 的核心机制。

1. 取消操作（Cancellation）

2. 设置超时（Timeout / Deadline）

3. 传递请求作用域的值（Request-scoped values）

context包如何实现goroutine的取消?
实现goroutine取消的cancelCtx结构体，其中的Context作为父context，done字段作为channel通道，children则是储存子context的map，当取消父context的时候，所有子context接收到channel的信号，实现级联取消。
