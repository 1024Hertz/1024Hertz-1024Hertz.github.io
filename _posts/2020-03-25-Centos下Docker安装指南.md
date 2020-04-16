---
title: Centos下Docker安装指南
tags:
  - Docker
  - centos
---

*  环境说明
> `CentOS 7`以后都可以安装`Docker`
~~~shell
cat /etc/redhat-release 
~~~
~~~shell
uname -a
~~~
---
* 安装准备
> 为了方便添加软件源，支持`devicemapper`存储类型
~~~shell
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
~~~
> 添加`Docker`稳定版本的`yum`软件源
~~~shell
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~
---
* 安装`Docker`
~~~shell
yum makecache fast
yum install docker-ce
~~~
---
* 加入`Docker`用户组命令
~~~shell
usermod -aG docker root
~~~
---
* 启动`Docker`
~~~shell
systemctl start docker
~~~
* 开机自启动
~~~shell
systemctl enable docker
~~~
---
* `Docker`配置
~~~shell
vim /etc/docker/daemon.json
~~~
~~~json
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"],
  "graph": "/usr/local/docker"
}
~~~
* `reload`配置文件
~~~shell
systemctl daemon-reload
~~~
* 重新启动`Docker`
~~~shell
systemctl restart docker.service
~~~
---
* 安装`docker-compose`
~~~shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose -version
~~~