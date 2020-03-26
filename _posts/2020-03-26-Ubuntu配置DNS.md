---
title: Ubuntu配置DNS
tags:
  - ubuntu
---

```shell
vim /etc/systemd/resolved.conf
```

![DNS](/assets/image/ubuntu/ubuntu-dns.png)

```shell
systemctl restart systemd-resolved.service
```