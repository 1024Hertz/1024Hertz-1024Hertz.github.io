---
title: Centos下Docker安装指南
tags:
  - Docker
  - centos
---

*  环境说明
```shell script
cat /etc/redhat-release 
```
> CentOS 7 以后都可以安装 Docker
```shell script
uname -a
```
---
* 安装准备
> 为了方便添加软件源，支持 devicemapper 存储类型
```shell script
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
> 添加 Docker 稳定版本的 yum 软件源
```shell script
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
---
* 安装 Docker
```shell script
yum makecache fast
yum install docker-ce
```
---
* 加入 docker 用户组命令
```shell script
usermod -aG docker root
```
---
* 启动Docker
```shell script
systemctl start docker
```
> 开启自启
```shell script
systemctl enable docker
```
---
* docker配置
```shell script
vim /etc/docker/daemon.json
```
```json
{
 "registry-mirrors": ["https://registry.docker-cn.com"] 
}
```
> reload配置文件
```shell script
systemctl daemon-reload
```
> 重新启动
```shell script
systemctl restart docker.service
```