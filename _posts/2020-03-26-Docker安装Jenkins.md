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
* 进入`/usr/local/docker/jenkins/home/`, 打开`hudson.model.UpdateCenter.xml`
* 将`url`替换为`https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json`
---
~~~shell
cd /usr/local/docker/jenkins/home/updates
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json
~~~

# 报错`No such plugin: cloudbees-folder`
* 打开链接`http://ftp.icm.edu.pl/packages/jenkins/plugins/cloudbees-folder/`
* 在最下面找到并打开`latest`目录
* 将目录中的`cloudbees-folder.hpi`下载
* 访问 IP:PORT/restart，越过配置插件的页面，直接访问
* 点击【系统管理】–【管理插件】–【高级】–【上传插件】，手动安装下载好的插件，即可

---
# Jenkins更新
* 以root用户进入jenkins容器
~~~shell
docker exec -it -u root jenkins /bin/bash
~~~
* 查看容器中jenkins war包的位置，并备份原来的war包
~~~shell
whereis jenkins
cd /usr/share/jenkins
cp jenkins.war jenkins.war.bak
~~~
* 将`/var/jenkins_home`下的包拷贝到`/usr/share/jenkins`下覆盖
~~~shell
cp /var/jenkins_home/jenkins.war.jar /usr/share/jenkins/
mv jenkins.war.jar jenkins.war
~~~
* 退出容器并重启