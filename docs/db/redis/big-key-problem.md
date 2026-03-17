> The Redis big key problem refers to a scenario where a key corresponds to a value that occupies a large amount of memory, leading to performance degradation, insufficient memory, data imbalance, and master-slave synchronization delays in Redis.


Typically, a key with a string-type value exceeding 1MB in size or a collection-type key containing more than 10,000 elements is considered a big key.

## Impacts of Big Keys

**Excessive Memory Usage**: Big keys consume a significant amount of memory, potentially leading to memory shortages and triggering eviction policies. In extreme cases, this could result in memory exhaustion, causing Redis instances to crash and affecting system stability.

**Performance Degradation**: Big keys occupy large amounts of memory, increasing memory fragmentation and impacting Redis performance. Operations on big keys—such as reads, writes, and deletions—consume more CPU time and memory resources, further reducing system performance.

**Blocking Other Operations**: Certain operations on big keys can block Redis instances. For example, executing the DEL command to delete a big key may render the Redis instance unresponsive to other client requests for a period of time, affecting response time and throughput.

**Network Congestion**: Retrieving big keys generates large amounts of network traffic, potentially saturating machine or local network bandwidth and impacting other services. For instance, if a big key is 1MB in size and accessed 1,000 times per second, it generates 1,000MB (1GB) of traffic.

**Master-Slave Synchronization Delays**: In Redis instances configured with master-slave synchronization, big keys can cause synchronization delays. Since big keys consume substantial memory, transferring them during synchronization results in increased network latency, affecting data consistency.

**Data Skew**: In Redis cluster mode, if one data shard consumes significantly more memory than others, it prevents balanced memory usage across shards. Additionally, reaching the maxmemory threshold defined in Redis may cause critical keys to be evicted, potentially leading to memory overflow.

## How to Identify Big Keys

### SCAN Command

By using the Redis SCAN command, all keys in the database can be gradually traversed. In combination with other commands (such as STRLEN, LLEN, SCARD, and HLEN), big keys can be identified. The advantage of SCAN is that it allows traversal without blocking the Redis instance.

### bigkeys Parameter

Using the redis-cli client, you can scan for the largest key in each data type by running the following command:
```bash
redis-cli -h 127.0.0.1 -p 6379 --bigkeys
```

### Redis RDB Tools

The open-source Redis RDB Tools can analyze RDB files to scan for big keys. For example, the following command outputs the top three keys that occupy more than 1KB of memory:
```bash
rdb --command memory --bytes 1024 --largest 3 dump.rdb
```

## How to Solve the Big Key Problem

**Split into Multiple Smaller Keys**: The simplest approach is to reduce the size of individual keys. Multiple keys can be read using MGET in batch operations.

**Data Compression**: When using the String type, applying compression algorithms can reduce value size. Alternatively, using the Hash type can help, as Redis stores small hash values efficiently using a compressed list data structure.

**Set Reasonable Expiration Times**: Assign expiration times to each key to ensure data is automatically cleared upon expiration, preventing long-term accumulation into big keys.
Enable Memory Eviction Policies: Activate Redis memory eviction strategies, such as Least Recently Used (LRU), so that the least-used data is automatically evicted when memory runs low, preventing big keys from occupying memory indefinitely.

**Data Sharding**: Implement Redis Cluster to distribute data across multiple Redis instances, reducing the burden on any single instance and mitigating the big key problem.

**Deleting Big Keys**: Use the UNLINK command to delete big keys asynchronously. Unlike DEL, UNLINK removes keys in the background, preventing Redis instances from being blocked.
