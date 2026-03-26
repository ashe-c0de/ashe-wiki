## 一、确认并开启慢查询日志

默认情况下，MySQL 的慢查询日志可能是关闭的。你需要先检查状态并开启它。

```sql
SHOW VARIABLES LIKE 'slow_query_log';
SHOW VARIABLES LIKE 'long_query_time'; -- 阈值，默认通常是 10 秒
SHOW VARIABLES LIKE 'slow_query_log_file'; -- 日志文件路径
```

### 1、临时开启

```sql
-- 开启慢查询日志
SET GLOBAL slow_query_log = 'ON';

-- 设置阈值（例如：超过 1 秒就算慢查询，单位是秒）
SET GLOBAL long_query_time = 1;

-- 记录没有使用索引的查询（强烈建议开启，能发现潜在问题）
SET GLOBAL log_queries_not_using_indexes = 'ON';
```

### 2、永久开启（修改配置文件）

编辑`my.cnf` (Linux) ，在 [mysqld] 下添加：

```
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/log/mysql/mysql-slow.log  # 确保目录有写权限
long_query_time = 1
log_queries_not_using_indexes = 1
```

## 二、分析慢查询日志

使用官方工具 mysqldumpslow

这是 MySQL 自带的工具，无需安装。它可以对日志进行归类汇总。

```sql
# 按返回记录集大小排序，取前 10 条
mysqldumpslow -s r -t 10 /var/log/mysql/mysql-slow.log

# 按访问次数排序，取前 10 条 (最常用，找出出现频率高的慢查询)
mysqldumpslow -s c -t 10 /var/log/mysql/mysql-slow.log

# 按总耗时排序，取前 10 条 (找出最拖慢系统的查询)
mysqldumpslow -s t -t 10 /var/log/mysql/mysql-slow.log

# 过滤包含特定关键字 (如 'select') 的查询
mysqldumpslow -s t -t 10 -g "select" /var/log/mysql/mysql-slow.log
```

## 慢查询排查路线

1. 开启 slow_query_log 和 log_queries_not_using_indexes。
2. 等待 业务运行一段时间，积累日志。
3. 运行 mysqldumpslow -s c -t 10 找出出现频率最高的慢 SQL。
4. 执行 EXPLAIN 分析该 SQL 的执行计划。
5. 优化 索引或 SQL 写法。
6. 验证 优化后的执行时间。
