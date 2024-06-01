---
title: ubuntu redis安装
date: 2023/7/8 19:46:25
comments: true
tags: 
- redis
- ubuntu
categories: 
- 计算机
- redis
---

# ubuntu安装redis

环境配置

安装gcc 和 make

```
sudo apt-get install -y gcc
sudo apt-get install -y g++
sudo apt-get install -y gcc automake autoconf libtool make
```

在Ubuntu中打开终端，输入下列命令，下载Redis安装包：

```
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
```

解压：

```cobol
tar xzf redis-4.0.9.tar.gz
```

 移动：

```cobol
sudo mv ./redis-4.0.9 /usr/local/redis/
```

进入redis⽬录，编译生成

命令：

```cobol
cd /usr/local/redis/
sudo make
```

安装编译后的redis放在 /usr/local/redis目录下

```
make install PREFIX=/usr/local/redis
```

可以启动redis了，默认启动模式为前台启动，执行如下

```
./redis-server
```

一般采用后台启动，否则客户端关闭，redis服务也就停止了

```
cp /usr/local/redis/redis.conf  /usr/local/redis/
```

修改redis.conf,将**daemonize no -> daemonize yes**

```
vim redis.conf
```

连接redis

```
cd bin
./redis-cli
ping
```