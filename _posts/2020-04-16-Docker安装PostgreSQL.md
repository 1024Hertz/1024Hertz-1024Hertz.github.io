---
title: Docker安装PostgreSQL
tags:
  - PostgreSQL
  - Docker
---

# 拉取镜像
~~~shell
docker pull postgres:latest
~~~

# 创建文件夹
~~~shell
mkdir -p /home/local/docker/postgresql/data
~~~

# 创建容器
~~~shell
docker run -d \
    --name=postgresql \
    -p 5432:5432 \
    -v /home/local/docker/postgresql/data:/var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=123456 \
    -e TZ=PRC \
    postgres:latest
~~~