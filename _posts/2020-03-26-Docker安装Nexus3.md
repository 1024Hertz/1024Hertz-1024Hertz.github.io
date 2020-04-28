---
    title: Docker安装Nexus3
tags:
  - Nexus3
  - Docker
---

# 搜索镜像
~~~shell
docker search sonatype/nexus3
~~~

# 安装镜像
~~~shell
docker pull sonatype/nexus3
~~~

# 创建文件夹
~~~shell
mkdir -p /usr/local/docker/nexus3/data \
&& chmod 777 /usr/local/docker/nexus3/data
~~~

# 自定义网络
~~~shell
docker network create --driver bridge --subnet=172.18.0.0/16 --gateway=172.18.0.1 customNetwork
~~~

# 启动镜像
~~~shell
docker run -it --privileged=true -d \
    --name nexus3 \
    --restart=always \
    -p 6000:8081 \
    --mount type=bind,source=/usr/local/docker/nexus3/data,target=/nexus-data \
    sonatype/nexus3
~~~

# 浏览器访问
* http://YourIP:6000
* Nexus 的默认帐号是`admin`, 密码是`admin123`
