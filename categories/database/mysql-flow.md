← [Ashe wiki](../../README.md)

---

## sql语句到MySQL的整体流程

```mermaid
graph TD
    Client[客户端] -->|1. 发送 SQL| Connector[连接器]
    Connector -->|2. 验证账号密码/权限| Analyzer[分析器]
    Analyzer -->|3. 词法/语法分析| Optimizer[优化器]
    Optimizer -->|4. 生成执行计划| Executor[执行器]
    Executor -->|5. 再次校验表级权限| StorageEngine[存储引擎 InnoDB/MyISAM]
    StorageEngine -->|6. 读写数据 磁盘/内存| Executor
    Executor -->|7. 返回结果集| Connector
    Connector -->|8. 返回结果| Client
```

理解这个流程对于 SQL 调优 至关重要：
- 连接慢？检查网络或连接池。
- 语法错？检查 SQL 写法。
- 执行慢但索引没走对？检查 优化器（统计信息是否准确，EXPLAIN 分析）。
- 执行慢且走了索引但还是慢？检查 存储引擎（磁盘 I/O、Buffer Pool 命中率、锁竞争）。
