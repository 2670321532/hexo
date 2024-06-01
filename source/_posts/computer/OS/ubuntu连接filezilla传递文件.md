---
title: ubuntu连接filezilla传文件
date: 2023/7/8 17:46:25
comments: true
tags: 
- ubuntu
categories: 
- 计算机
- ubuntu
---

## windows下载客户端

https://www.filezilla.cn/download/client

## ubuntu安装 ftp

### 1.1 安装 ftp

```
sudo apt-get install vsftpd
```

### 1.2 安装完成以后使用如下 vi 命令打开/etc/vsftpd.conf

```
sudo vi /etc/vsftpd.conf
```

打开以后 vsftpd.conf 文件以后找到如下两行：

```
local_enable=YES
write_enable=YES
```

没有就忽略，有的就确保上面两行前面没有“#”，有的话就取消掉，修改完 vsftpd.conf 以后保存退出

### 1.3 重启 FTP 服务

```
sudo /etc/init.d/vsftpd restart
```

赶紧打开 filezilla，通过 ftp 协议连接吧



## ubuntu安装 SSH

### 查看 ssh 服务是否开启

```
ps -e |grep ssh
```

### 安装 ssh 服务

如果你用的是redhat，fedora，centos等系列linux发行版，那么敲入命令sudo yum install sshd 或者 sudo yum install openssh-server

如果你使用的是debian，ubuntu，linux mint等系列的linux发行版，那么敲入 sudo apt-get install sshd 或者 sudo apt-get install openssh-server 然后按照提示，安装就好了。

### 2.3 开启 ssh 服务

```
sudo service sshd start
```

