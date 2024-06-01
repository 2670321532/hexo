---
title: hexo使用
date: 2013/7/13 20:46:25
comments: true
tags: 
- blog
- hexo
categories: 
- 计算机
- hexo
---

# 运行和安装

```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

# 生成

使用 Hexo 生成静态文件非常简单快捷。

```
$ hexo generate
```

### 注意文件更改

Hexo可以监视文件更改并立即重新生成文件。Hexo 将比较文件的 

SHA1 校验和，并且仅在检测到文件更改时才写入。

```
$ hexo generate --watch
```

### 生成后部署

若要在生成后进行部署，可以运行以下命令之一。两者之间没有区

别。

```
$ hexo generate --deploy
$ hexo deploy --generate
```

# 端口号占用如何处理

在Linux中，可以使用以下命令来[查看端口号](https://so.csdn.net/so/search?q=查看端口号&spm=1001.2101.3001.7020)的占用情况：

```
sudo netstat -tlnp
```

该命令会列出当前所有正在使用的端口号以及占用该端口号的进程

的信息。

如果需要释放某个端口号，可以使用以下命令：

```
sudo kill <进程ID>
```

其中，进程ID是占用该端口号的进程的唯一标识符。可以通过上述 

netstat 命令来查看进程ID。

如果进程ID不知道，也可以使用以下命令来释放该端口号：

```
sudo fuser -k <端口号>/tcp
```

这个命令会终止占用该端口号的进程。需要注意的是，如果占用端口号的进程是系统关键进程或正在运行的重要程序，不要轻易终止它。另外，在使用 kill 或 fuser 命令时，一定要小心，确保不会意外终止其他进程。netstat -tlnp 命令用于列出系统上所有正在使用的TCP和UDP端口，并显示每个端口对应的进程信息。

### 重新生成和部署

在确保配置文件正确后，运行以下命令重新生成并部署网站：

```
bash复制代码cd D:\blog
hexo clean
hexo generate
hexo deploy
```