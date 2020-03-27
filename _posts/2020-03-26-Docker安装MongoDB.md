---
title: Docker安装MongoDB
tags:
  - MongoDB
  - Docker
---

# 拉取镜像
~~~shell
docker pull mongo:latest
~~~

# 创建文件夹
~~~shell
mkdir -p /usr/local/docker/mongo/data/configdb \
&& mkdir -p /usr/local/docker/mongo/data/db
~~~

# 创建容器
~~~shell
docker run -d \
    --name mongod_27017 \
    -p 27017:27017  \
    --network customNetwork --ip 172.18.0.10 \
    -v /usr/local/docker/mongo/data/configdb:/data/configdb/ \
    -v /usr/local/docker/mongo/data/db:/data/db/ \
    mongo:latest \
    --auth 
~~~

# 设置账号和密码
~~~shell
docker exec -it mongod_27017 mongo admin
db.createUser({ user: 'admin', pwd: '123456', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });	
~~~

# 设置一个其他用户
~~~shell
db.auth("admin", "123456")
db.createUser({ user: 'olympos', pwd: '123456', roles: [ { role: "readWrite", db: "olympos" } ] });
~~~

# 查看容器IP
~~~shell
docker exec -it mongod_27017 cat /etc/hosts
~~~

	 
