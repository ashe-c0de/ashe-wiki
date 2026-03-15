## Overview

Elasticsearch是一种分布式、面向文档的NoSQL数据库，专门设计用于全文搜索、数据分析和日志存储。它不仅可以存储大量文档数据，还支持复杂的全文搜索、数据聚合和分析查询，使其非常适用于日志管理、搜索引擎、实时分析和大规模数据存储等场景。

Elasticsearch的数据结构由索引`Index`、文档`Document`和字段`Field`组成，数据结构如下所示

```graph
Index 1 (e.g., "products")
   └─ Document 1 (with ID "1")
   │    ├─ Field 1 (e.g., "product_name": "Laptop")
   │    ├─ Field 2 (e.g., "price": 999.99)
   │    └─ Field 3 (e.g., "description": "High-performance laptop")
   │
   └─ Document 2 (with ID "2")
        ├─ Field 1 (e.g., "product_name": "Smartphone")
        ├─ Field 2 (e.g., "price": 499.99)
        └─ Field 3 (e.g., "description": "Feature-rich smartphone")
```

关系型数据库（MySQL）与ElasticSearch的结构对比

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-mysql.png)

> [Elasticsearch 8.0.0 no longer supports mapping types.](https://www.elastic.co/docs/manage-data/data-store/mapping/removal-of-mapping-types)

> version 8.0.0以后不再支持type的映射，因此我们可以简单理解：Index等同于MySQL中表的概念

> 流程：创建Index，在指定Index中创建Document，Document则由Field组成

> 关系型数据库通常使用Navicat这样的工具来操作数据库，对于ElasticSearch则可以使用Postman等接口请求工具来操作（当然kibana也可以）

## Index

```bash
# 创建名为products的Index
PUT http://ip:9200/products

# 查询所有Index
GET http://ip:9200/_cat/indices?v

# 查询指定Index
GET http://ip:9200/products

# 删除Index
DELETE http://ip:9200/products 
```

## Document

```bash
# 插入文档
curl --location --request POST 'http://ip:9200/products/_doc/001' \
--header 'Content-Type: application/json' \
--data '{
    "title": "ElasticSearch",
    "content": "hello world",
    "version": "001"
}'
```

```bash
# 修改文档（修改指定字段值）Update API 主要用于部分更新（partial updates）文档。它允许你仅更新文档的一部分字段。
curl --location --request POST 'http://ip:9200/products/_update/001' \
--header 'Content-Type: application/json' \
--data '{
    "doc": {
        "title": "kibana"
    }
}'
```

```bash
# 修改文档 Index API 通常用于创建新文档或完全替换（replace）现有文档
curl --location --request PUT 'http://ip:9200/products/_doc/001' \
--header 'Content-Type: application/json' \
--data '{
    "title": "kibana 2",
    "content": "welcome to elasticsearch",
    "version": "0_0"
}'
```

```bash
# 根据文档ID查询文档
curl --location --request GET 'http://ip:9200/products/_doc/001'
```

```bash
# 查询全部文档 
curl --location --request GET 'http://ip:9200/products/_search'
```

```bash
# 根据文档ID删除文档
curl --location --request DELETE 'http://ip:9200/shopping/_doc/001'
```

## Query

```bash
# 条件查询 criteria query
GET /index/_search
{
  "query": {
    "match": {
      "field_name": "value"
    }
  }
}

# 分页查询 paged query
GET /index/_search
{
  "from": 0,          
  "size": 10,
  "query": {
    "match": {
      "field_name": "value"
    }
  }
}

# 排序查询 sorted query
GET /index/_search
{
    "from": 0,
    "size": 10,
    "query": {
        "match": {
            "field_name": "value"
        },
        "sort": {
            "field_name": "asc"
        }
    }
}
```

在全文搜索引擎和信息检索系统中，ElasticSearch的优秀表现得益于其倒排索引(Inverted Index)，向Index中插入一个Document时，分词器对Document进行分词得到词汇/词条(term)，对于每个词条，倒排索引维护一个文档列表，其中包含了包含该词条的文档的引用（例如文档ID或位置信息Document在磁盘中的地址？）

```bash
Document 1 (with ID "1")
  ├─ Field 1 (e.g., "quote": "Why so serious")
  ├─ Field 2 (e.g., "author": "Joker")
  └─ Field 3 (e.g., "description": "享乐的生活")
Document 2 (with ID "2")
  ├─ Field 1 (e.g., "quote": "Glory is fleeting, but obscurity is forever.")
  ├─ Field 2 (e.g., "author": "Napoleon")
  └─ Field 3 (e.g., "description": "荣誉的生活")
Document 3 (with ID "3")
  ├─ Field 1 (e.g., "quote": "I think, therefore I am")
  ├─ Field 2 (e.g., "author": "Descartes")
  └─ Field 3 (e.g., "description": "沉思的生活")
  
# 以上3个文档插入Index时，分词器对description字段进行分词，得到了5个term(词汇/词条)：享乐、荣誉、沉思、的、生活，并维护了term和文档ID的关系
    key             value
——————————————————————————
    享乐            [1]
    荣誉            [2]
    沉思            [3]
    的              [1,2,3]
    生活            [1,2,3]

# 显然，当检索term时，即可从倒排索引中快速获取到对应的文档ID以及文档在磁盘中的位置。
```