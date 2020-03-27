---
title: Centos下JDK安装指南
tags:
  - JDK
  - centos
---

* 解压JDK
~~~shell
mkdir /usr/local/jdk
tar -zxvf jdk-8u241-linux-x64.tar.gz 
~~~
> 配置
~~~shell
vim /etc/profile
~~~
~~~
# JDK环境配置
export JAVA_HOME=/usr/local/jdk/jdk1.8.0_241
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
~~~
~~~shell
source /etc/profile
~~~
