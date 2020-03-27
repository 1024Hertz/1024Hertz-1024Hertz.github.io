---
title: Docker安装Redis
tags:
  - Redis
  - Docker
---

# 拉取镜像
~~~shell
docker pull redis:latest
~~~

# 创建文件夹
~~~shell
mkdir -p /usr/local/docker/redis/data
~~~

# 配置项
~~~
replicaof <masterip> <masterport>
~~~

# 创建容器
> master
~~~shell
docker run -itd  \
  --name  redis_master \
  -p 63791:6379 \
  --net customNetwork --ip 172.18.0.20 \
  -v /usr/local/docker/redis/data/redis_master.conf:/data/redis_master.conf \
  --privileged=true \
  redis:latest \
  redis-server /data/redis_master.conf
~~~
> slave
~~~shell
docker run -itd  \
  --name  redis_slave \
  -p 63792:6379 \
  --net customNetwork --ip 172.18.0.21 \
  -v /usr/local/docker/redis/data/redis_slave.conf:/data/redis_slave.conf \
  --privileged=true \
  redis:latest \
  redis-server /data/redis_slave.conf
~~~
* `redis-server –appendonly yes` : 在容器执行redis-server启动命令，并打开redis持久化配置

# 哨兵服务
* `wget http://download.redis.io/redis-stable/sentinel.conf`
~~~shell
docker run -itd \
  --name sentinel_1 \
  -v /usr/local/docker/redis/data/sentinel_31.conf:/data/sentinel_31.conf \
  --net customNetwork --ip 172.18.0.31 \
  redis:latest \
  redis-sentinel /data/sentinel_31.conf
~~~
~~~shell
docker run -itd \
  --name sentinel_2 \
  -v /usr/local/docker/redis/data/sentinel_32.conf:/data/sentinel_32.conf \
  --net customNetwork --ip 172.18.0.32 \
  redis:latest \
  redis-sentinel /data/sentinel_32.conf
~~~
~~~shell
docker run -itd \
  --name sentinel_3 \
  -v /usr/local/docker/redis/data/sentinel_33.conf:/data/sentinel_33.conf \
  --net customNetwork --ip 172.18.0.33 \
  redis:latest \
  redis-sentinel /data/sentinel_33.conf
~~~

# 
~~~shell
docker inspect redis-master | grep IPA
docker exec -it redis_master redis-cli
~~~
* 进入redis客户端，再输入密码，然后使用info查看节点信息
    * 