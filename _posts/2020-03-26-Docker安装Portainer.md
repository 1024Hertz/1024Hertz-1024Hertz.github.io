---
title: Docker安装Portainer
tags:
  - Portainer
  - Docker
---

# 安装镜像
~~~shell
docker pull portainer/portainer
~~~

# 启动容器
~~~shell
docker run -it -d \
    -p 7010:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --name prtainer \
    portainer/portainer
~~~

# 浏览器访问
* http://192.168.216.99:7010/#/home
* admin 12345678