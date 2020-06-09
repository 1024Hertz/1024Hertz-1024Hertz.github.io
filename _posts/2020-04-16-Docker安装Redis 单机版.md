---
title: Docker安装Redis 单机版
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
mkdir -p /usr/local/redis/{conf,data}
~~~

# 获取redis的默认配置模板
~~~shell
wget https://raw.githubusercontent.com/antirez/redis/5.0/redis.conf -O conf/redis.conf

sed -i 's/logfile ""/logfile "access.log"/' conf/redis.conf
sed -i 's/appendonly no/appendonly yes/' conf/redis.conf
sed -i 's/bind 127.0.0.1/bind 0.0.0.0/' conf/redis.conf
sed -i 's/protected-mode yes/protected-mode no/' conf/redis.conf
~~~

# 创建容器
~~~shell
docker run -it -d \
  --privileged=true \
  --name centos_redis \
  -p 8518:6379 \
  -v /usr/local/redis/data:/data \
  -v /usr/local/redis/conf/redis.conf:/etc/redis/redis.conf \
  redis:latest \
  redis-server /etc/redis/redis.conf
~~~