context（上下文）是 控制并发操作生命周期、传递请求范围数据 的核心机制。

取消操作（Cancellation）

设置超时（Timeout / Deadline）

传递请求作用域的值（Request-scoped values）

context包如何实现goroutine的取消?

实现goroutine的取消是cancelCtx结构体，其中的Context字段作为父context，done字段作为channel通道，children字段则是储存子context的map，当取消父context的时候，所有子context接收到channel的信号，实现goroutine的级联取消。

context包如何实现goroutine的超时？

实现goroutine的超时是timerCtx结构体，其内部是cancelCtx字段，timer以及deadline，timer用于触发超时，deadline用于调用链端共享的截止时间，timer会在超时后调用cancelCtx的cancel方法，进而取消goroutine。