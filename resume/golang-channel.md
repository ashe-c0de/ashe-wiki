← [Ashe wiki](../README.md)

---

无缓冲 Channel 是实现 Goroutine 之间强同步的最佳方式，它确保了数据发送和接收操作同时发生。[代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/channel/goroutine-communication.go)

> context.WithTimeout就是计时器结合无缓冲channel实现的

有缓冲 Channel 在发送和接受操作时是非阻塞的，除非缓冲已满（发送操作阻塞）或缓冲空了（接受操作阻塞）。  

channel + goroutine 通常会像是一个协程分发任务（生产者），多个协程（消费者）会并发地消费任务。  
> 相对而言，使用协程（是用空间换时间），不使用协程（是用时间换空间，假设耗时t_0），这就引申出使用协程的判断依据，当一个任务拆分
> 后，创建协程和分配任务的时间t_1 + 最慢的子任务耗时t_2 + 结果汇总（Channel 传输/锁竞争）的时间t_3 = t_total，如果t_total没有> 明显小于t_0，那么这个任务的拆分就是在白忙活。


select就是多路复用监听channels的作用，如果所有 channel 都关闭了且没有数据了，select 会立即选中 default（如果有）


```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

// cookTask: 模拟任务
// 关键点 1: 任务结束后，必须 defer close(ch) 通知接收方“数据流结束了”
func cookTask(name string, duration time.Duration, ch chan<- string) {
	// 确保即使发生 panic，channel 也会被关闭（防止接收方死锁）
	defer close(ch)

	time.Sleep(duration)
	ch <- fmt.Sprintf("%s 做好了!", name)
	// 函数结束，defer 执行，channel 自动关闭
}

func main() {
	rand.Seed(time.Now().UnixNano())

	// 1. 创建 channel
	chNoodle := make(chan string)
	chSteak := make(chan string)
	chBread := make(chan string)

	// 2. 启动任务
	go cookTask("煮面", time.Duration(rand.Intn(3)+1)*time.Second, chNoodle)
	go cookTask("煎牛排", time.Duration(rand.Intn(3)+1)*time.Second, chSteak)
	go cookTask("烤面包", time.Duration(rand.Intn(3)+1)*time.Second, chBread)

	fmt.Println("👨‍🍳 厨师们开始做菜了...")
	fmt.Println("🤵 服务员开始监听...")

	// 关键点 2: 维护一个“活跃通道”计数器
	// 初始有 3 个 channel 需要监听
	activeChannels := 3

	// 关键点 3: 循环监听，直到 activeChannels 变为 0
	for activeChannels > 0 {
		select {
		// 关键点 4: 使用 comma-ok 惯用语法接收数据
		// msg: 接收到的数据
		// ok: 如果 channel 未关闭且有数据，ok=true; 如果 channel 已关闭且无数据，ok=false
		case msg, ok := <-chNoodle:
			if !ok {
				// channel 已关闭，不再监听它
				chNoodle = nil // 将 channel 置为 nil，select 会自动跳过 nil 的 case
				activeChannels--
				fmt.Println("🍜 煮面通道已关闭 (剩余任务:", activeChannels, ")")
				continue // 跳过本次循环剩余逻辑，直接进入下一次 select
			}
			fmt.Println("🍜 收到消息:", msg)

		case msg, ok := <-chSteak:
			if !ok {
				chSteak = nil
				activeChannels--
				fmt.Println("🥩 煎牛排通道已关闭 (剩余任务:", activeChannels, ")")
				continue
			}
			fmt.Println("🥩 收到消息:", msg)

		case msg, ok := <-chBread:
			if !ok {
				chBread = nil
				activeChannels--
				fmt.Println("🍞 烤面包通道已关闭 (剩余任务:", activeChannels, ")")
				continue
			}
			fmt.Println("🍞 收到消息:", msg)
		}
	}

	fmt.Println("✅ 所有菜都上齐了，所有通道已关闭，服务员优雅下班！")
}
```
