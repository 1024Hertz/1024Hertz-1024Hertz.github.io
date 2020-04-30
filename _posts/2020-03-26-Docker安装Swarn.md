---
title: Docker安装Swarn
tags:
  - Swarn
  - Docker
---

# 启动Swarn
> Docker 默认包含了 Swarm，因此可以直接使用
~~~shell
docker swarm init --advertise-addr 192.168.216.99
~~~

# 查看Swarn信息
~~~shell
docker node ls
~~~