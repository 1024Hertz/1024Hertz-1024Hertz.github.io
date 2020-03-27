---
title: git push github 免输入账号和密码方法
tags:
  - git
  - github
---

* git config --global credential.helper store
* 打开~/.gitconfig文件，会发现多了一项:
~~~properties
[credential]
helper = store
~~~
* 此时,再次push  输入用户名和密码,以后再次push即可免去输入用户名和密码
