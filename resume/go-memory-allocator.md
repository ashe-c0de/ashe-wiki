← [Ashe wiki](../README.md)

---

> 当我们在讨论go的内存分配器（Memory Allocator）的时候，其内存特指堆内存（Heap）

```bash
为什么 Go 需要自己的内存分配器？
1️⃣ 系统调用成本高
直接使用操作系统的 malloc/free，每次分配都进入内核。

2️⃣ 锁竞争严重
多线程同时分配内存会竞争锁。

3️⃣ 性能不可控

因此go的内存分配器目的是为了：减少锁竞争、提高分配速度、降低内存碎片、配合 GC 高效工作。
```


Go 的堆内存管理采用经典的 三级缓存结构，从快到慢依次为：
- mcache（线程本地缓存）
- mcentral （全局中间层）
- mheap（真正的堆）

Go 内存分配器采用 mcache → mcentral → mheap 的三级结构，通过 P 私有缓存 + size class + span 批量分配，在高并发 goroutine 环境下实现低锁竞争的高效内存分配。


| 组件 | 归属 | 锁机制 | 作用 | 访问速度 |
| :--- | :--- | :--- | :--- | :--- |
| mcache | 每个 P (逻辑处理器) 独占 | 无锁 (Lock-free) | 存储常用小对象的空闲链表。P 上的 G 分配内存时首选这里。 | ⚡️ 极速 |
| mcentral | 全局共享 (按规格分类) | 加锁 (Mutex) | 当 mcache 缺货时，从这里获取一批同规格的内存块填充 mcache。 | 🚗 中速 |
| mheap | 全局共享 | 加锁 (Mutex) | 管理所有的大页内存。当 mcentral 也没货时，从这里分配大的 span。 | 🐢 慢速 |

小对象：mcache → (miss) → mcentral → (miss) → mheap  
大对象：(判断 size > 32KB) → 直接 mheap (跳过 mcache 链表查找和 mcentral)
