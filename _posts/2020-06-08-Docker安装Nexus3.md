---
title: Docker安装Nexus3
tags:
  - Maven
  - Nexus
  - Docker
---

# 拉取镜像
~~~shell
docker pull sonatype/nexus3
~~~
---
# 创建文件夹
~~~shell
mkdir -p /usr/local/nexus3/nexus-data
~~~
---
# 启动容器
~~~shell
docker run -d -it \
    --privileged=true \
    --name=centos_nexus3 \
    --restart=always \
    -p 8118:8081 \
    -v /usr/local/nexus3/nexus-data:/nexus-data \
    sonatype/nexus3
~~~
---
# 修改密码
~~~shell
docker exec -it centos_nexus3 bash
cd /nexus-data/
cat admin.password 
~~~
---
## 修改文件`settings.xml`
~~~shell
<servers>
   <server>
       <id>nexus3</id>
       <username>admin</username>
       <password>admin</password>
   </server>
</servers>
~~~