---
title: Docker安装Jenkins
tags:
  - Jenkins
  - Docker
---

# 安装镜像
~~~shell
docker pull jenkins/jenkins:lts
~~~

# 创建文件夹
~~~shell
mkdir -p /usr/local/docker/jenkins/home
~~~

# 自定义网络
~~~shell
docker network create --driver bridge --subnet=172.18.0.0/16 --gateway=172.18.0.1 customNetwork
~~~

# 启动镜像
~~~shell
docker run -it -d \
    --name jenkins \
    --user=root \
    -p 7000:8080 \
    -p 7070:50000 \
    -v /usr/local/docker/jenkins/home:/var/jenkins_home \
    jenkins/jenkins:lts
~~~

# 查找密码
~~~shell
docker exec -it jenkins /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword
~~~
