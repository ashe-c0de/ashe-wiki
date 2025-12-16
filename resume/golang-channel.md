← [Ashe wiki](../README.md)

---

无缓冲 Channel 是实现 Goroutine 之间强同步的最佳方式，它确保了数据发送和接收操作同时发生。（context.WithTimeout就是计时器结合无缓冲channel实现的）  
有缓冲 Channel 在发送和接受操作时是非阻塞的，除非缓冲已满（发送操作阻塞）或缓冲空了（接受操作阻塞）。  
有channel + goroutine 构成的像是一个“并发安全的内存级别ＭＱ”，多个协程（消费者）会并发地消费消息。  
