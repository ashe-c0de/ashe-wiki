← [Ashe wiki](../README.md)

---
Redis 的淘汰策略（Eviction Policy）是指当 Redis 内存使用量达到配置的上限（maxmemory）时，如何选择哪些数据删除，以便为新写入的数据腾出空间。
> 如果未配置 maxmemory（默认情况），Redis 会尝试使用所有可用内存，直到操作系统杀死进程（Out Of Memory）。因此，生产环境必须配置 maxmemory 并选择合适的淘汰策略。

| maxmemory-policy策略                  | 说明                                             |
| ------------------- | ---------------------------------------------- |
| **noeviction**      | 不淘汰数据，内存满时写操作直接返回错误（默认策略）。                     |
| **volatile-lru**    | 从 **设置了过期时间的 key** 中，淘汰 **最近最少使用（LRU）** 的 key。 |
| **allkeys-lru**     | 从 **所有 key** 中，淘汰 **最近最少使用（LRU）** 的 key。(最常用)       |
| **volatile-lfu**    | 从 **设置了过期时间的 key** 中，淘汰 **使用频率最低（LFU）** 的 key。 |
| **allkeys-lfu**     | 从 **所有 key** 中，淘汰 **使用频率最低（LFU）** 的 key。       |
| **volatile-random** | 从 **设置了过期时间的 key** 中随机淘汰。                      |
| **allkeys-random**  | 从 **所有 key** 中随机淘汰。                            |
| **volatile-ttl**    | 从 **设置了过期时间的 key** 中淘汰 **剩余 TTL 最短** 的 key。    |

可以按 两维度 理解：

1️⃣ 淘汰范围

volatile-*：只在 有过期时间的 key 中淘汰

allkeys-*：在 所有 key 中淘汰

2️⃣ 淘汰算法

lru：最近最少使用 （Least Recently Used）

lfu：使用频率最低 (Least Frequently Used)

random：随机

ttl：TTL 最短

| 配置状态 | maxmemory | maxmemory-policy | 内存满时的表现 | 谁触发的？ |
| :--- | :--- | :--- | :--- | :--- |
| 默认情况 | 未设置 (0) | (不生效) | 进程被操作系统杀死 (OOM) | 操作系统 (Kernel) |
| 受控情况 | 已设置 (如 2G) | noeviction (默认) | 写操作返回错误，进程存活 | Redis 自身 |
| 受控情况 | 已设置 (如 2G) | allkeys-lru | 自动删除旧数据，写入成功，进程存活 | Redis 自身 |


