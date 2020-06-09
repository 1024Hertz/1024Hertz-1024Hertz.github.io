---
title: Docker容器中安装VIM
tags:
  - Docker
  - VIM
---

# 备份
~~~shell
mv /etc/apt/sources.list /etc/apt/sources.list.bak
~~~
---
# 替换源
~~~shell
echo "deb http://mirrors.163.com/debian/ jessie main" >/etc/apt/sources.list \
&& echo "deb http://mirrors.163.com/debian/ jessie-updates main" >>/etc/apt/sources.list
~~~
---
# 更新源
~~~shell
apt-get update 
~~~
---
# 安装VIM
~~~shell
apt-get install vim
~~~ 