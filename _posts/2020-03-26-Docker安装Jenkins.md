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
mkdir -p /usr/local/jenkins/home
~~~

# 启动镜像
~~~shell
docker run -it -d \
    --privileged=true \
    --name centos_jenkins \
    --restart=always \
    --user=root \
    -p 8318:8080 \
    -p 8328:50000 \
    -e JAVA_OPTS=-Duser.timezone=Asia/Shanghai \
    -v /etc/localtime:/etc/localtime \
    -v /usr/local/jenkins/home:/var/jenkins_home \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /usr/bin/git:/usr/bin/git \
    -v /usr/local/jdk:/usr/local/jdk \
    -v /usr/local/maven:/usr/local/maven \
    jenkins/jenkins:lts
~~~
---
# 更改镜像地址
~~~shell
https://jenkins-zh.cn/tutorial/management/plugin/update-center/
进入/usr/local/jenkins/home/, 打开hudson.model.UpdateCenter.xml
将url替换为https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
~~~
---
~~~shell
cd /usr/local/jenkins/home/updates
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json
~~~
---
# 查找密码
~~~shell
docker exec -it jenkins /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword
~~~

# 报错`No such plugin: cloudbees-folder`
* 打开链接
~~~shell
http://ftp.icm.edu.pl/packages/jenkins/plugins/cloudbees-folder/
~~~
* 在最下面找到并打开`latest`目录
* 将目录中的`cloudbees-folder.hpi`下载
* 访问 IP:PORT/restart，越过配置插件的页面，直接访问
* 点击【系统管理】–【管理插件】–【高级】–【上传插件】，手动安装下载好的插件，即可

---
# Jenkins更新
* 以root用户进入jenkins容器
~~~shell
docker exec -it -u root centos_jenkins /bin/bash
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
