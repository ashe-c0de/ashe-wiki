多版本并发控制(Multiversion Concurrency Control)
是一种通过维护数据多个版本来消除读写冲突的技术。

在传统的双锁协议（Two-Phase Locking，简称 2PL）下，当某一行数据在进行写操作的时候，并发的读操作就会发生阻塞，也就是读写冲突，mvcc就是为了解决这个问题而推出的方案。（在读多写少的场景下，MVCC 能极大地提高并发性能，因为没有大量的锁等待和死锁问题。）

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/db-mvcc.png)

| t | trx1 | trx2 |
|----------|-----------|-------------|
| t0 | update t set 余额 = 0 where id = 1; |  |
| t1 |  | select 余额 from t where id = 1; |
| t2 | commit; |  |
| t3 |  | select 余额 from t where id = 1; |

如上所示，两个并发的事务发生时。在读已提交（Read Committed）的事务隔离机制下，事务2的两次查询结果分别为10、0。  
在重复读（Repeatable Read）的事务隔离机制下，事务2的两次查询结果都是10。

当事务1的第二个sql不是commit而是rollback，那么数据库该如何回滚呢，这就涉及到MySQL在每张表中的隐藏字段db_trx_id(事务ID)、db_roll_ptr（回滚指针）……  
以及MySQL的undolog会记录所有数据的修改操作，并通过db_roll_ptr维护好链表（Multiversion多版本就是一行数据多次修改记录的版本链）。 
 
| 当前版本 | db_trx_id | db_roll_ptr |
|----------|-----------|-------------|
| New Version | 101 | → Old Version |
| Old Version | 99 | → Older Version |
| Older Version | ... | → ... |

并发读写时，read view正是基于db_trx_id & 数据库当前的事务隔离机制来决定读取结果。
> MVCC = undo log + 版本链 + read view