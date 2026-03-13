## 1. 拉取 Redis 镜像

使用以下命令拉取 Redis 6.2 版本镜像（选了一个较为稳定的版本）：

```bash
docker pull redis:6.2
```

## 2、创建配置文件
比如在服务器的用户目录下/home/w/（俺的服务器用户目录）
```bash
mkdir redis_cluster
cd redis_cluster
mkdir bin
mkdir conf
mkdir data
cd data
mkdir node-1
mkdir node-2
mkdir node-3
mkdir node-5
mkdir node-6
```
conf目录创建配置文件vim redis-node-1.conf ，依次6个文件redis-node-1.conf  redis-node-2.conf  redis-node-3.conf  redis-node-4.conf  redis-node-5.conf  redis-node-6.conf
```bash
bind 0.0.0.0
protected-mode no
cluster-enabled yes
cluster-config-file nodes-7000.conf
cluster-node-timeout 5000
cluster-announce-ip 192.168.1.6
cluster-announce-port 7000
cluster-announce-bus-port 17000

user r_user on >read123 ~* +@read ~* -@write ~* -@admin
user rw_user on >write123 ~* +@read ~* +@write ~* -@admin
user admin on >admin123 ~* &* +@all
user default off
```

## 3、创建脚本
创建初始化脚本 vim init_redis.sh 
```bash
#!/bin/bash

echo "Creating Redis cluster..."

# 启动6个Redis节点
for i in {1..6}; do
  port=$((6999 + $i))
  container="redis-node-$i"
  config="/home/w/redis/conf/redis-node-$i.conf"
  data="/home/w/redis/data/node-$i"

  docker run -d --name $container \
    -p $port:6379 -p $((port + 10000)):16379 \
    --net redis-cluster \
    -v $config:/usr/local/etc/redis/redis.conf \
    -v $data:/data \
    -e TZ="Asia/Shanghai" \
    --ulimit nofile=100000:100000 \
    redis:6.2 redis-server /usr/local/etc/redis/redis.conf

  # 检查容器是否启动成功
  if [ $? -ne 0 ]; then
    echo "Failed to start $container"
    exit 1
  fi
done

# 等待节点启动
sleep 10

# 获取所有节点的 IP，这里是容器IP
nodes=""
for i in {1..6}; do
  ip=$(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' redis-node-$i)
  nodes+="$ip:6379 "
done

echo "Nodes: $nodes"

# 创建 Redis 集群（使用密码连接）
docker exec -it redis-node-1 redis-cli -u redis://admin:admin123@localhost:6379 --cluster create $nodes --cluster-replicas 1

echo "All Redis nodes should be running now. Here is the list of running containers:"
docker ps | grep redis
```

创建停止脚本 vim stop_redis.sh
```bash
#!/bin/bash

# 停止redis cluster
for i in {1..6}; do
  docker stop redis-node-$i
done
```

创建删除镜像脚本 vim rm_redis.sh
```bash
#!/bin/bash

for i in {1..6}; do
  docker rm redis-node-$i
done
```

创建启动容器脚本 vim start_redis.sh 
```bash
#!/bin/bash

for i in {1..6}; do
  docker start redis-node-$i
done
```

查看集群状态信息

docker exec -it redis-node-1 redis-cli CLUSTER INFO

带密码版

docker exec -it redis-node-1 redis-cli -u redis://admin:admin123@localhost:6379 CLUSTER INFO
![我的图片](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/redis-cluster-01.png)



查看集群节点信息

docker exec -it redis-node-1 redis-cli CLUSTER NODES

docker exec -it redis-node-1 redis-cli -u redis://admin:admin123@localhost:6379 CLUSTER NODES

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/redis-cluster-02.png)


最后通过客户端连接测试

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/redis-cluster-03.png)


<br><br>
后记：不得不说Redis Cluster集群还是很消耗CPU和带宽资源的，因为我的服务器是自己的旧笔记本搭建，启动后系统的CPU和网络资源负载很大（怪不得Redis应用一般独占一台服务器资源）

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/redis-cluster-04.png)
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/redis-cluster-05.png)