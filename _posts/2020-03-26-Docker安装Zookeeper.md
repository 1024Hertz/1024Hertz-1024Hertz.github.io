---
title: Docker安装Zookeeper
tags:
  - Zookeeper
  - Docker
---

# 搜索镜像
```shell script
docker search zookeeper
```

# 安装镜像
```shell script
docker pull zookeeper:latest
```

# 创建文件夹
```shell script
mkdir -p /usr/local/docker/zookeeper/data \
&& mkdir -p /usr/local/docker/zookeeper/conf
```

# 自定义网络
```shell script
docker network create --driver bridge --subnet=172.18.0.0/16 --gateway=172.18.0.1 customNetwork
```

# 启动镜像
```shell script
docker run -it --privileged=true -d \
    --name zookeeper_2181 \
    --network customNetwork --ip 172.18.0.2 \
    -p 2181:2181 \
    -v /usr/local/docker/zookeeper/data:/data \
    -v /usr/local/docker/zookeeper/conf:/conf \
    zookeeper:latest
```

# 使用 ZK 命令行客户端连接
```shell script
docker run -it --rm \
    --link zookeeper_2181:zookeeper \
    zookeeper:latest \
    zkCli.sh -server zookeeper
```

# 检查容器的运行情况
```shell script
docker exec -it zookeeper_2181 /bin/bash
```

# 查看日志
```shell script
docker logs -f zookeeper_2181
```
