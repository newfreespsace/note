---
title: Linux
updated: 2022-11-23 00:57:18Z
created: 2022-09-15 08:10:50Z
latitude: 31.46749500
longitude: 104.67894600
altitude: 0.0000
---

# Linux 
## 创建用户
```bash
# adduser kk //添加用户kk
# adduser -g root kk //添加用户kk，并指定用户所在的组为root用户组
# adduser -r kk //创建一个系统用户kk
# adduser -d /home/myf kk //新添加用户kk，其home目录为/home/myf
//当用户名kk登录主机时，系统进入的默认目录为/home/myf
```
## 修改密码
```bash
passwd kk
```
## ssh root 登录
```bash
sudo vim /etc/ssh/sshd_config
```
修改 配置 PermitRootLogin yes
重启
```bash
sudo service ssh restart
```


### 修改 ip 地址
* 获取网卡名，比如说是 eno1
* 编辑文件 ``/etc/entplan/50-cloud-init.yml``
```yml
network:
  version: 2
   ethernets:
     eno1:
       addresses: [10.38.1.13/24]
       gateway4: 10.38.1.1
       dhcp4: false
       nameservers:
         addresses: [61.139.2.69]
```