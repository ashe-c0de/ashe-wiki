## 分布式锁的核心机制：

- NX (Not Exists)：仅在 Key 不存在时才设置成功。这保证了只有一个请求能拿到锁。
- PX (Expiration)：设置过期时间（如 30,000 毫秒）。防止某个请求挂掉后，锁永远不释放导致“死锁”。
- unique_value：每个请求生成的随机唯一 ID。释放锁时需校验该值，防止“误删”他人的锁。

## 如何实现可重入锁？

todo

## 如何实现自动续约？

为了保证请求的行为执行完毕，启一个协程去自动续约（延长key的过期时间），当行为执行完会调用unlock删除key（并通过context去cancel该协程避免无限续约），确保行为执行完成再释放锁。

[Go代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/devops/distributed-lock.go)
