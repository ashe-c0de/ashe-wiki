golang的GMP模型是用于实现高并发调度的核心机制。它通过 G（goroutine）+ M（machine）+ P（processor） 三个角色协同工作，使 Go 可以高效运行成千上万个 goroutine。

- Goroutine（协程）
- Processor（逻辑处理器）
- Machine（系统线程）


```graph
HTTP 请求到达
      ↓
创建 goroutine (G_new, 由 go runtime 创建)
      ↓
加入 P 本地队列
      ↓
若本地队列满 → 转入 Global Queue
      ↓
M + P 调度执行
      ↓
若 P 队列空 → 从 Global Queue 取
      ↓
若 Global Queue 也空 → Work Stealing
      ↓
执行请求逻辑
      ↓
请求完成（G_new生命周期结束）

        +---- P0 ----+
        | G1 G2 G3   |
        +------------+
             ↑
             |
             M0

        +---- P1 ----+
        | G4 G5 G6   |
        +------------+
             ↑
             |
             M1
```

> P 的数量 = 默认等于 CPU 核数，可以通过设置`GOMAXPROCS`配置  
> M + P -> 才能运行 G  
> P.localQueue -> G -> M 执行  
