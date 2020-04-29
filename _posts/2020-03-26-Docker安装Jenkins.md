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
docker run -it --privileged=true -d \
    --name jenkins \
    --restart=always \
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

# 更改镜像地址
* `https://jenkins-zh.cn/tutorial/management/plugin/update-center/`
* 需要你进入jenkins的工作目录, 打开`hudson.model.UpdateCenter.xml`
* 将`url`替换为`https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json`

# 报错`No such plugin: cloudbees-folder`
* 打开链接`http://ftp.icm.edu.pl/packages/jenkins/plugins/cloudbees-folder/`
* 在最下面找到并打开`latest`目录
* 将目录中的`cloudbees-folder.hpi`下载
* 访问 IP:PORT/restart，越过配置插件的页面，直接访问
* 点击【系统管理】–【管理插件】–【高级】–【上传插件】，手动安装下载好的插件，即可