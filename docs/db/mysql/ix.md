MySQL 中常见的索引类型：

【按逻辑结构分类】

- 主键索引（PRIMARY KEY）  
    - 唯一、非空。  
    - 每张表只能有一个。  
    - InnoDB 中，主键索引是聚簇索引（Clustered Index），数据行按主键物理存储。

- 唯一索引（UNIQUE）  
    - 索引列的值必须唯一（允许有 NULL，但通常只允许一个 NULL）。  
    - 可用于保证数据唯一性（如身份证号、邮箱）。

- 普通索引（INDEX / KEY）
    - 单列索引（Single-column Index）  
    - 联合索引（Composite Index/Compound Index）

最左匹配原则（Leftmost Prefix Principle）

> 联合索引在使用时要注意到索引失效的情况，比如给table的字段a, b, c建立联合索引。
> 索引就像一本字典，先按 a 排序，a 相同再按 b 排序，b 相同再按 c 排序。你必须先确定 a，才能利用 b 的有序性。
> 当查询条件跳过a，那么b和c字段的索引都是失效的。

- 全文索引（FULLTEXT）  
    - 用于对大文本字段（如 `CHAR`、`VARCHAR`、`TEXT`）进行关键词搜索。  
    - 支持 `MATCH() AGAINST()` 语法。  
    - 仅 MyISAM 和 InnoDB（5.6+）支持。  
    - 不适用于精确匹配或范围查询。

- 空间索引（SPATIAL）  
    - 用于地理空间数据类型（如 `GEOMETRY`、`POINT`、`POLYGON`）。  
    - 仅 MyISAM 和 InnoDB（5.7+）支持。  
    - 使用 R 树结构存储。