← [Ashe wiki](../README.md)

---
什么是索引覆盖？
- 一个查询所需的所有数据（包括 SELECT 列、WHERE 条件列、JOIN 列、ORDER BY 列等）都可以直接从索引树中获取，而无需回表（即不需要再去读取原始的数据行/聚簇索引）。


在 MySQL 的 InnoDB 引擎中，数据存储结构分为两种：
聚簇索引（Clustered Index）：通常是主键索引，叶子节点存储的是整行数据。
二级索引（Secondary Index / 非聚簇索引）：叶子节点存储的是索引列的值 + 主键值。
