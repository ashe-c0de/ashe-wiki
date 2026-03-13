```go
type hchan struct {
    qcount   uint           // 当前队列中已有的元素个数（即 channel 中待接收的数据量）
    dataqsiz uint           // 环形缓冲区的容量（即 make(chan T, N) 中的 N）
    buf      unsafe.Pointer // 指向底层环形缓冲区数组的指针
    elemsize uint16         // 单个元素的大小（字节）
    closed   uint32         // 通道是否已关闭的标志（0:未关闭, 1:已关闭）
    elemtype *_type         // 元素的类型信息（用于反射和类型检查）
    sendx    uint           // 发送索引：下一个数据要写入缓冲区的位置
    recvx    uint           // 接收索引：下一个数据要从缓冲区读取的位置
    recvq    waitq          // 等待接收数据的 goroutine 队列（链表）
    sendq    waitq          // 等待发送数据的 goroutine 队列（链表）

    lock mutex              // 互斥锁：保护上述所有字段，保证并发安全
}
```

无缓冲 Channel 是实现 Goroutine 之间强同步的最佳方式，它确保了数据发送和接收操作同时发生。[代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/channel/goroutine-communication.go)

> context.WithTimeout就是计时器结合无缓冲channel实现的

有缓冲 Channel 在发送和接受操作时是非阻塞的，除非缓冲已满（发送操作阻塞）或缓冲空了（接受操作阻塞）。  

重复close channel会发生panic
> panic: close of closed channel

往未初始化的channel（nil）中发送数据，会发生error
> fatal error: all goroutines are asleep - deadlock!

channel + goroutine 通常会像是一个协程分发任务（生产者），多个协程（消费者）会并发地消费任务。  
> 相对而言，使用协程（是用空间换时间），不使用协程（是用时间换空间，假设耗时t_0），这就引申出使用协程的判断依据，当一个任务拆分
> 后，创建协程和分配任务的时间t_1 + 最慢的子任务耗时t_2 + 结果汇总（Channel 传输/锁竞争）的时间t_3 = t_total，如果t_total没有> 明显小于t_0，那么这个任务的拆分就是在白忙活。


select就是多路复用监听channels的作用，如果所有 channel 都关闭了且没有数据了，select 会立即选中 default（如果有）
[代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/channel/go-select.go)