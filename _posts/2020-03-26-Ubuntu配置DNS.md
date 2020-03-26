---
title: Ubuntu防火墙
tags:
  - ubuntu
---

```shell script
vim /etc/systemd/resolved.conf
```
![1561360717822](assets/image/ubuntu/ubuntu-dns.png)
```shell script
sudo systemctl restart systemd-resolved.service
```