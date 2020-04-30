---
title: Centos下Docker安装指南
tags:
  - Docker
  - centos
---

*  环境说明
> `CentOS 7`以后都可以安装`Docker`
~~~shell
cat /etc/redhat-release 
~~~
~~~shell
uname -a
~~~
---
* 安装准备
> 为了方便添加软件源，支持`devicemapper`存储类型
~~~shell
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
~~~
> 添加`Docker`稳定版本的`yum`软件源
~~~shell
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~
---
* 安装`Docker`
~~~shell
yum makecache fast
yum install docker-ce
~~~
---
* 加入`Docker`用户组命令
~~~shell
usermod -aG docker root
~~~
---
* 启动`Docker`
~~~shell
systemctl start docker
~~~
* 开机自启动
~~~shell
systemctl enable docker
~~~
---
* `Docker`配置
~~~shell
vim /etc/docker/daemon.json
~~~
~~~json
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"],
  "graph": "/usr/local/docker"
}
~~~
* `reload`配置文件
~~~shell
systemctl daemon-reload
~~~
* 重新启动`Docker`
~~~shell
systemctl restart docker.service
~~~
---
* 安装`docker-compose` 方式一
~~~shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose -version
~~~
---
* 安装`docker-compose` 方式二
~~~shell
https://github.com/docker/compose/releases
mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose -version
~~~

# Docker开启Remote API 访问 2375端口
~~~shell
vim /usr/lib/systemd/system/docker.service
~~~
---
~~~shell
[Service]
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
~~~
## 重启
~~~shell
systemctl daemon-reload
systemctl restart docker
~~~
---
## 安全配置
* `Docker`守护进程的主机上，生成`CA`私有和公共密钥
~~~shell
openssl genrsa -aes256 -out ca-key.pem 4096
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
~~~
* 创建一个服务器密钥和证书签名请求`CSR`
~~~shell
openssl genrsa -out server-key.pem 4096
openssl req -subj "/CN=192.168.216.99" -sha256 -new -key server-key.pem -out server.csr
~~~
* `CA`来签署公共密钥:
~~~shell
vim extfile.cnf
~~~
---
~~~shell
subjectAltName = DNS:192.168.211.238,IP:192.168.211.238,IP:127.0.0.1
extendedKeyUsage = serverAuth
~~~
* 生成`key`
~~~shell
openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
~~~
* 创建客户端密钥和证书签名请求:
~~~shell
openssl genrsa -out key.pem 4096
openssl req -subj '/CN=client' -new -key key.pem -out client.csr
~~~
* 修改`extfile.cnf`
~~~shell
subjectAltName = DNS:192.168.216.99,IP:192.168.216.99,IP:127.0.0.1
extendedKeyUsage = clientAuth
~~~
* 生成签名私钥：
~~~shell
openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile.cnf
~~~
> client.csr和server.csr两个文件可以删除
* Docker服务停止
> 启动Docker守护进程，并将其挂载在2375端口
~~~shell
sudo dockerd --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem -H=0.0.0.0:2375
~~~
---
## 验证远程端口
~~~shell
docker -H tcp://123.123.123.123:2375 version
~~~
---
## 客户端使用证书连接
* 方式一
> 复制ca.pem,cert.pem,key.pem三个文件到客户端
~~~shell
docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem -H=192.168.216.99:2375 version
~~~
* 方式二
> 默认连接该Docker服务器  
> 将ca.pem,cert.pem,key.pem三个文件移动到 ~/.docker  
> 设置环境变量  
~~~shell
export DOCKER_HOST=tcp://$HOST:2375 DOCKER_TLS_VERIFY=1
~~~

---

# 参考资料
* https://docs.docker.com/engine/security/https/
