---
title: Docker安装MySQL5.x
tags:
  - 数据库
  - MySQL
  - Docker
---

# 拉取镜像
~~~shell
docker pull mysql:5.7
~~~
---
# 启动容器
~~~shell
docker run -d \
  --name centos_mysql_5.7.x \
  -p 8418:3306 \
  -v /etc/localtime:/etc/localtime \
  -e MYSQL_ROOT_PASSWORD=root \
  mysql:5.7 \
  --lower_case_table_names=1 \
  --character-set-server=utf8mb4 \
  --collation-server=utf8mb4_unicode_ci
~~~
---
# 进入容器
~~~shell
docker exec -it centos_mysql_5.7.x bash
~~~
---
# 设置容器自动启动
~~~shell
docker update --restart=always centos_mysql_5.7.x
~~~
---
# `/etc/mysql/mysql.conf.d/mysqld.cnf`
~~~cfg
[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
symbolic-links=0
lower_case_table_names=1
sql_mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
~~~
---
# 官方帮助文档
[MySQL 5.7 Docker](https://hub.docker.com/r/cytopia/mysql-5.7/)