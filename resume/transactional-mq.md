← [Ashe wiki](../README.md)

---
什么是事务消息

1. 发送 Half Message → Broker
2. 执行本地事务 → 返回 Commit / Rollback / Unknown
   ├─ Commit → 消息对消费者可见
   ├─ Rollback → 删除消息
   └─ Unknown → 进入第3阶段
3. Broker 定期回查事务状态 → 最终确定 Commit 或 Rollback

事务消息选择放弃强一致性，保证最终一致性。以实现数据库写入和发送MQ的强绑定关系。

Tags: [#other](../tags/map.md)
