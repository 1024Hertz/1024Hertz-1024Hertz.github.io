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
---
* 创建自定义网络
~~~shell
docker network create -d bridge --subnet 192.1.0.0/16 network_1
~~~
~~~shell
docker network inspect network_1
~~~
---
* 从容器里面拷文件到宿主机
> 在宿主机里面执行以下命令
~~~shell
docker cp 容器名:要拷贝的文件在容器里面的路径 要拷贝到宿主机的相应路径 
~~~
---
* 从宿主机拷文件到容器里面
> 在宿主机里面执行以下命令
~~~shell
docker cp 要拷贝的文件路径 容器名:要拷贝到容器里面对应的路径
~~~
---