---
title: Ubuntu配置DNS
tags:
  - ubuntu
---

```
vim /etc/systemd/resolved.conf
```

![1561360717822](assets/image/ubuntu/ubuntu-dns.png)

```
sudo systemctl restart systemd-resolved.service
```