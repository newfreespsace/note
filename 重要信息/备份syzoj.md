---
title: 备份syzoj
updated: 2022-09-17 04:56:14Z
created: 2022-09-01 01:43:05Z
---

### 备份数据库

1.进入系统，获取管理员身份

```bash
sudo -i
```

2.键入命令，备份数据库

```bash
mysqldump -uroot syzoj >  /home/lee/syzoj.sql
```

上面命令将数据库``syzoj`` 备份至 ``/home/lee/syzoj.sql``

3.导出数据库至本地计算机

```bash
scp lee@192.168.50.128:/home/lee/syzoj.sql C:\Users\Zt4lxer\
```

上面命令将文件从远程计算机复制到本地。

### 还原数据库

1.将本地备份文件复制到远程计算机

```bash
scp .\syzoj.sql lee@192.168.50.128:
```

2.新建和原备份数据库同名数据库。

```bash
create database syzoj;
```

3.导入数据库

```bash
mysql -uroot syzoj < syzoj.sql
```

### 备份数据

1.将整个 ``/etc`` 目录下的文件全部打包成为 ``/tmp/etc.tar``

```bash
[root@linux ~]# tar -cvf /tmp/etc.tar /etc <==仅打包，不压缩！
[root@linux ~]# tar -zcvf /tmp/etc.tar.gz /etc <==打包后，以 gzip 压缩
[root@linux ~]# tar -jcvf /tmp/etc.tar.bz2 /etc <==打包后，以 bzip2 压缩
```

对于本 ``oj``，比如我们想把``opt/syzoj/`` 下的 ``data`` 文件夹打包至``/home/lee``

```bash
tar -zcvf /home/lee/data.tar.gz data
```

2.解压

```bash
tar -xvf /usr/local/test.tar
```

tar 解压缩命令详解

```bash
1 -c: 建立压缩档案
2 -x：解压
3 -t：查看内容
4 -r：向压缩归档文件末尾追加文件
5 -u：更新原压缩包中的文件
```

解压到指定目录

```bash
tar -xvf test.tar -C /tmp
```

本 ``oj`` 中

```bash
tar -xvf data.tar -C /opt/syzoj/
```







### 利用 ``iptables`` 实现端口转发

1. 将本地的端口转发到本机端口

   ```bash
   iptables -t nat -A PREROUTING -p tcp --dport 7777 -j REDIRECT --to-port 6666
   ```

2. 查看当前 ``iptables`` 的 ``nat`` 表的所有规则：（不用 -t 指定表名默认的是指 filter 表）

   ```bash
   iptables -t nat -nL --line
   ```

3. 删除该端口转发

   ```bash
   iptables -t nat -D PREROUTING  <序号>
   ```

4. 将本机的端口转发到其他机器

   ```bash
   # 通过 1.168 的 6666 端口访问 1.8 的 7777 端口，在 1.168 上设置
   sysctl -w net.ipv4.ip_forward=1
   # 5000
   iptables -t nat -A PREROUTING -p tcp --dport 5000 -j DNAT --to-destination 10.38.0.10:5000
   iptables -t nat -A POSTROUTING -p tcp -d 10.38.0.10 --dport 5000 -j SNAT --to-source 10.38.1.13
   # 3389
   iptables -t nat -A PREROUTING -p tcp --dport 3389 -j DNAT --to-destination 10.38.0.10:3389
   iptables -t nat -A POSTROUTING -p tcp -d 10.38.0.10 --dport 3389 -j SNAT --to-source 10.38.1.13
   # 2222
   iptables -t nat -A PREROUTING -p tcp --dport 2222 -j DNAT --to-destination 10.38.0.10:2222
   iptables -t nat -A POSTROUTING -p tcp -d 10.38.0.10 --dport 2222 -j SNAT --to-source 10.38.1.13
   # 3596
   iptables -t nat -A PREROUTING -p tcp --dport 3596 -j DNAT --to-destination 10.38.0.10:3596
   iptables -t nat -A POSTROUTING -p tcp -d 10.38.0.10 --dport 3596 -j SNAT --to-source 10.38.1.13
   
   ```
   
   

#### mysql 修改密码

```mysql
ALTER USER 'root'@'localhost' identified by '123456' ; ”

flush privileges;
# 刷新
```


####  常用软件
mathpix








npx tsc -b
