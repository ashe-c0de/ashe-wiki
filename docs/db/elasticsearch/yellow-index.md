> 在Elasticsearch中，索引的健康状态（health status）反映了索引的分片分配情况和集群的整体健康状况。这些状态可以帮助您快速了解索引和集群的运行情况。以下是Elasticsearch中索引的三种健康状态及其意义：

> 1. green（绿色）
> 含义：所有主分片（primary shards）和副本分片（replica shards）都已成功分配到集群中的节点（node）上。
> 特点：
> 集群中的所有数据都是完全可用的。
> 所有主分片和副本分片都已成功分配。
> 集群处于最佳状态，具有最高的冗余和容错能力。


> 2. yellow（黄色）
> 含义：所有主分片都已成功分配，但至少有一个副本分片未被分配。
> 特点：
> 集群中的所有数据都是可读写的，但不是所有的副本分片都已分配。
> 这种状态通常是由于集群中的节点数量不足，无法满足副本分片的分配要求。
> 虽然数据是安全的，但容错能力降低，因为某些数据只有主分片而没有副本分片。


> 3. red（红色）
> 含义：至少有一个主分片未被分配。
> 特点：
> 集群中的部分数据不可用，可能导致搜索结果不完整或写入操作失败。
> 主分片未分配的原因可能是节点故障、网络问题或配置错误。
> 这种状态下，集群的稳定性和数据完整性受到严重影响，需要立即采取措施进行修复。

以上说明中存在一些术语——主分片（primary shards）和副本分片（replica shards），对于Elasticsearch集群而言，每个索引（index）的数据会被分成主分片（primary shards）和副本分片（replica shards），并且分布在不同的节点上。

也就是说，Elasticsearch希望数据的主分片和副本分片具有高可用性，当某一节点故障时，其节点上数据A的主分片失效，其他节点（数据A）的副本分片仍然可以正常使用。

---
*例子：*
*在3个节点的集群环境下，假设您创建了一个index，配置为3个主分片和1个副本分片：*

*主分片（primary shards）：P0, P1, P2*  

*副本分片（replica shards）：R0, R1, R2*  

*那么插入某一个document的分片可能会被分配如下：*  

*节点1：P0, R1*  
*节点2：P1, R2*  
*节点3：P2, R0*  

*注：副本分片是相对主分片而言的，因此当存在3个主分片，每个主分片都会有自己的1个副本分片，共计3个副本分片。*

---

由此可知，当Elasticsearch上index的健康状态为yellow时，说明至少有一个副本分片未被分配（这种情况下，数据仍然可以正常使用，但是失去了高可用性的保障）


理解以上概念后，现在处理问题（我的Elasticsearch集群单节点是安装在本地Windows主机上的）：

查看index列表

https://localhost:9200/_cat/indices

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/yellow-index-1.png)

可以看到除new_index以外，其他index都是yellow状态，为啥会出现这些区别呢？

因为products，users这两个index在创建索引的时候没有显式指定主分片数和副本分片数，Elasticsearch默认分配了1个主分片和1个副本分片，但是我的Elasticsearch只有一个节点，根据其分配算法，主分片和副本分片不应该在一个节点，因此副本分片分配节点时失败。

my_index是在创建时，显式指定了主分片数为3，副本分片数为1，给1个副本分片分配节点时失败。

new_index在创建时，显式指定了主分片数为1，副本分片数为0，因此这个index的健康状态是green。

cat_index在创建是，显式指定了主分片数为1，副本分片数为1，给1个副本分片分配节点时失败。

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/yellow-index-2.png)

```bash
curl -u "elastic:your_password" -k -X PUT "https://127.0.0.1:9200/cat_index" -H "Content-Type: application/json" -d "{\"settings\":{\"number_of_shards\":1,\"number_of_replicas\":1}}"
```

现在我准备修改my_index和cat_index的副本分片数为0，使得其健康状态恢复成green

```bash
curl -u "elastic:your_password" -k -X PUT "https://localhost:9200/my_index/_settings" -H "Content-Type: application/json" -d "{ \"index\": { \"number_of_replicas\": 0 }}"
```

```bash
curl -u "elastic:your_password" -k -X PUT "https://localhost:9200/cat_index/_settings" -H "Content-Type: application/json" -d "{ \"index\": { \"number_of_replicas\": 0 }}"
```

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/yellow-index-3.png)

现在我把所有index的副本分片数都修改为0后（所有index的健康状态都已经为green），再检查Elasticsearch集群的健康状态

https://localhost:9200/_cluster/health

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/yellow-index-4.png)

总结：在创建索引的时候，其配置的主分片数和副本分片数，要与你使用的Elasticsearch集群的节点数契合。

参考：[elasticsearch 未分配分片unassigned导致集群为Yellow状态修复](https://www.cnblogs.com/chenjw-note/articles/10956057.html)
