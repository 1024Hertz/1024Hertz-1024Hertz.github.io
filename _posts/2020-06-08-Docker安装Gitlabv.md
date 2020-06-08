---
title: Docker安装Gitlab
tags:
  - Gitlab
  - Docker
---

# 拉取镜像
~~~shell
docker pull gitlab/gitlab-ce
~~~
---
# 创建文件夹
~~~shell
mkdir -p /usr/local/gitlab/config \
mkdir -p /usr/local/gitlab/logs \
mkdir -p /usr/local/gitlab/data
~~~
---
# 启动容器
~~~shell
docker run -it -d \
  --privileged=true \
  --hostname gitlab.docker.com \
  --publish 8218:443 --publish 8228:80 --publish 8238:22 \
  --name centos_gitlab \
  --restart=always \
  --volume /usr/local/gitlab/config:/etc/gitlab \
  --volume /usr/local/gitlab/logs:/var/log/gitlab \
  --volume /usr/local/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
~~~
# 登陆
* 浏览器进入`http://ip:port`，使用`root`账户登录并设置密码即可进入管理员界面
---
# 升级
* 重复以上命令
* 建议先备份一下`/usr/local/gitlab/`