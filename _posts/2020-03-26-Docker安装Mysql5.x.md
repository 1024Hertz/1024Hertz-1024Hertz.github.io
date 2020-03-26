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
mkdir -p /usr/local/docker/mysql/conf \
&& mkdir -p /usr/local/docker/mysql/logs \
&& mkdir -p /usr/local/docker/mysql/data 
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
  mysql:5.7 \
  --lower_case_table_names=1 \
  --character-set-server=utf8mb4 \
  --collation-server=utf8mb4_unicode_ci
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
# `/etc/mysql/mysql.conf.d/mysqld.cfg`
```cfg
[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
symbolic-links=0
lower_case_table_names=1
sql_mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
```
---
# 官方帮助文档
[MySQL 5.7 Docker](https://hub.docker.com/r/cytopia/mysql-5.7/)