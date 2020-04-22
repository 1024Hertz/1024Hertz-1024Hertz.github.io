---
title: Centos下Docker操作指南
tags:
  - Docker
  - centos
---

* 查看容器的网络`ip`
> 查看某个容器得`ip`
~~~shell
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
~~~
> 查看所有容器的`ip`
~~~shell
docker inspect --format='{{.Name}} - {{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
~~~