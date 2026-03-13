什么是索引覆盖？

- 一个查询所需的所有数据（包括 SELECT 列、WHERE 条件列、JOIN 列、ORDER BY 列等）都可以直接从索引树中获取，而无需回表。

> **所谓回表，查询过程中通过索引的B+树查询，但是叶子节点中没有存储非索引列的值，于是根据主键ID，到聚簇索引查询整行数据，再获取具体列的值。（也就是查询过程即使走了索引，但是仍然再根据主键ID再查一次）**


在 MySQL 的 InnoDB 引擎中，数据存储结构分为两种：

- 聚簇索引（Clustered Index）：通常是主键索引，叶子节点存储的是整行数据。  
- 二级索引（Secondary Index / 非聚簇索引）：叶子节点存储的是索引列的值 + 主键值。  


索引覆盖就是为了解决回表现象的解决方案。

> 如果查询的列全部都包含在某个二级索引中，那么 MySQL 直接从这个二级索引里就能拿到所有需要的数据，完全不需要去查聚簇索引。

比如 users 表，建立联合索引 idx_name_age (name, age)
```sql
SELECT name, age FROM users WHERE name = 'Alice';
```
idx_name_age 索引节点里存有 name 和 age，就不用根据主键ID回表查询。（将要查询的字段和where条件的字段加上联合索引，使得叶子节点上的索引值可以覆盖select的字段，这种解决方案就叫索引覆盖）

---

理解索引覆盖的前提是：认识InnoDB存储引擎的两种索引结构——聚簇索引（Clustered Index） & 二级索引（Secondary Index）  

> 聚簇索引叶子节点存储的是 **主键值 & row(record的完整记录)**，因此`where id = 1`这样的查询直接就能获取该行的每个字段值，也就不存在回表的说法。  
> 二级索引叶子节点存储的是**二级索引列值 & 主键值**，因此`select col(非索引列) from t where index_col = xxx;`这样的查询必须回表，经由聚簇索引再查询一次。

```graph
B+Tree (Clustered Index)

非叶子节点
   ↓
[ key | pointer ]

叶子节点
   ↓
[ 主键值 | col1 | col2 | col3 | ... | colN ]




二级索引 B+Tree

非叶子节点
   ↓
[ key | pointer ]

叶子节点
   ↓
[ 二级索引列值 | 主键值 ]
```