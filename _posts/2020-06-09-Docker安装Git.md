---
    title: Docker安装Git
tags:
  - Git
  - Docker
---

# 源码安装
## 依赖包检查
~~~shell
rpm -q --qf '%{NAME}-%{VERSION}-%{RELEASE} (%{ARCH})\n' \
wget \
gcc-c++ \
zlib-devel \
perl-ExtUtils-MakeMaker
~~~

## 源码下载
~~~shell
https://mirrors.edge.kernel.org/pub/software/scm/git/
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.26.2.tar.gz
~~~

## 源码构建
~~~shell

~~~

