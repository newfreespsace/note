---
title: ssh 无法连接时
updated: 2023-03-08 12:47:12Z
created: 2023-03-08 12:45:40Z
latitude: 30.57281600
longitude: 104.06680100
altitude: 0.0000
---

ssh 无法连接时
在 .ssh 文件下新建 config 文件
```bash
Host github.com
Hostname ssh.github.com
Port 443
User git
```