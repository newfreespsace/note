---
title: syzoj 系统修改
updated: 2023-02-22 07:57:10Z
created: 2022-09-17 01:49:46Z
latitude: 31.46749500
longitude: 104.67894600
altitude: 0.0000
---

# SYZOJ
## systemd
在 etc/systemd/system 下新建 service 文件，用来开机时自动启动
syzoj-web、syzoj-judge-runner、syzoj-judge-daemon

## 评测机安装系统依赖
```bash
apt install build-essential libboost-all-dev
apt install rabbitmq-server redis-server
```
## 新增评测选项
https://github.com/syzoj/judge-v3/tree/master/src/languages
把 cpp17 复制一份，替换一下
index.ts 里面加一个
需要在 judge 端重新 yarn build
然后网页端配置相应改一改
https://github.com/syzoj/syzoj/blob/master/language-config.json
## 修改编译时间限制
修改 config 下的 runner-shared.json 文件

## 安装评测机时报错
可以尝试
```bash
apt-get install libboost-filesystem-dev
```