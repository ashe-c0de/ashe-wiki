# 🔍 grep 常用命令速查指南

`grep` 是 Linux 中最常用的文本搜索工具，用于在文件或标准输出中快速定位匹配内容，特别适合日志分析、数据排查、JSONL 文件检索。

---

## 1. 基本用法

| 命令 | 说明 |
| :--- | :--- |
| `grep "text" file` | 在文件中搜索文本 |
| `grep "text" *` | 在当前目录所有文件中搜索 |
| `grep -r "text" .` | 递归搜索当前目录 |

---

## 2. 最常用参数

| 参数 | 说明 |
| :--- | :--- |
| `-r` | 递归搜索子目录 |
| `-n` | 显示行号 |
| `-i` | 忽略大小写 |
| `-v` | 反向匹配（不包含） |
| `-l` | 只显示文件名 |
| `-c` | 统计匹配行数 |
| `-w` | 精确匹配单词 |
| `-o` | 只输出匹配内容 |

---

## 3. 日志排查常用组合

### 基础日志搜索

```bash
grep "ERROR" app.log
```

---

### 带行号（强烈推荐）

```bash
grep -n "ERROR" app.log
```

---

### 递归搜索整个目录

```bash
grep -rn "ERROR" .
```

---

### 忽略大小写

```bash
grep -rin "error" .
```

---

### 只看文件名（快速定位）

```bash
grep -rl "ERROR" .
```

---

### 排除某些内容

```bash
grep -rv "DEBUG" app.log
```

---

## 4. 进阶用法（非常常用）

### 只显示匹配内容（不显示整行）

```bash
grep -o "ERROR" app.log
```

---

### 统计匹配次数

```bash
grep -c "ERROR" app.log
```

---

### 显示上下文（日志排查神器）

```bash
grep -C 3 "ERROR" app.log
```

| 参数 | 说明 |
| :--- | :--- |
| `-C 3` | 上下各3行 |
| `-A 3` | 后3行 |
| `-B 3` | 前3行 |

---

## 5. 管道组合

### 多条件过滤

```bash
grep -rn "id123" . | grep "持仓盘中盯盘监控"
```

---

### 保存到文件

```bash
grep -rn "id123" . | grep "持仓盘中盯盘监控" > /tmp/result.txt
```

---

## 6. JSON / JSONL 专用技巧

### 查 JSONL 关键字段

```bash
grep '"task"' *.jsonl
```

---

### 查 UUID

```bash
grep "e05106a3-6372-4b9f-98c6-dc66f677b6c4" *.jsonl
```

---

### 查嵌套内容

```bash
grep "持仓盘中盯盘监控(实时)" *.jsonl
```

---

### 过滤 + 精确字段组合

```bash
grep -rn "id123" . | grep "持仓盘中盯盘监控"
```

---

## 7. 大文件优化技巧（很重要）

### 防止卡顿

```bash
LC_ALL=C grep -rn "keyword" .
```

👉 提升 grep 速度（关闭本地化影响）

---

### 忽略错误输出

```bash
grep -rn "keyword" . 2>/dev/null
```

---

### 限制文件类型

```bash
grep -rn --include="*.jsonl" "keyword" .
```

---

## 8. 查找 + 排序 + 去重

### 去重结果

```bash
grep "ERROR" app.log | sort | uniq
```

---

### 统计 Top N

```bash
grep "ERROR" app.log | sort | uniq -c | sort -nr | head
```

---

## 9. 最常用组合（建议背下来）

### 日志排查黄金组合

```bash
grep -rn "ERROR" . | less
```

---

### JSONL 排查

```bash
grep -rn --include="*.jsonl" "keyword" . > /tmp/out.txt
less /tmp/out.txt
```

---

### 精准定位

```bash
grep -rn "uuid" . | grep "业务关键词"
```

---

## 10. 高频 Top 8（运维必备）

| 命令 | 作用 |
| :--- | :--- |
| `grep -rn` | 最常用搜索 |
| `grep -rl` | 找文件 |
| `grep -C 3` | 上下文 |
| `grep -i` | 忽略大小写 |
| `grep -v` | 排除 |
| `grep -o` | 只显示匹配 |
| `grep -c` | 计数 |
| `grep \| less` | 分页查看 |

---

## 🚀 一句话总结

> grep = 日志世界的“搜索引擎”，less = “浏览器”，vim = “编辑器”
