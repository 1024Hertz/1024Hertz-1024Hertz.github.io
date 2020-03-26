---
title: Docker安装MySQL5.x
tags:
  - 数据库
  - MySQL
  - Docker
---

# 拉取镜像
```shell script
docker pull mysql:5.7
```
---
# 创建文件夹
```shell script
mkdir /usr/local/docker/mysql/conf \
&& mkdir /usr/local/docker/mysql/logs \
&& mkdir /usr/local/docker/mysql/data 
```
---
# 启动容器
```shell script
docker run -d \ 
  --name mysql-5.7.x \
  -p 3306:3306 \
  -v /etc/localtime:/etc/localtime \
  -v /usr/local/docker/mysql/conf:/etc/mysql/conf.d \
  -v /usr/local/docker/mysql/logs:/logs \
  -v /usr/local/docker/mysql/data:/mysql_data \
  -e MYSQL_ROOT_PASSWORD=root \
  --lower_case_table_names=1 \
  --character-set-server=utf8mb4 \
  --collation-server=utf8mb4_unicode_ci \
  mysql:5.7 
```
---
# 进入容器
```shell script
docker exec -it mysql-5.7.x bash
```
---
# 设置容器自动启动
```shell script
docker update --restart=always mysql-5.7.x
```
---
# 官方帮助文档
[MySQL 5.7 Docker](https://hub.docker.com/r/cytopia/mysql-5.7/)