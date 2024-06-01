---
title: 文件储存系统搭建
date: 2023/7/8 19:46:25
comments: true
tags: 
- FastDFS
- docker
categories: 
- 计算机
- docker
---

# Docker安装部署FastDFS

### docker拉取镜像

```
docker search fastdfs

root@feelapple-virtual-machine:/usr/local/nginx# docker search fastdfs
NAME                           DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
season/fastdfs                 FastDFS                                         88                   
ygqygq2/fastdfs-nginx          整合了nginx的fastdfs                                33                   [OK]
luhuiguo/fastdfs               FastDFS is an open source high performance d…   25                   [OK]
morunchang/fastdfs             A FastDFS image                                 20                   
delron/fastdfs                                                                 17                   
qbanxiaoli/fastdfs             FastDFS+FastDHT(单机+集群版)                         15                   [OK]
moocu/fastdfs                  fastdfs5.11                                     9                    
dodotry/fastdfs                更新到最新版本，基于Centos8/nginx1.19.8/Fast…             6                    
ecarpo/fastdfs-storage                                                         4                    
ecarpo/fastdfs                                                                 3                    
imlzw/fastdfs-tracker          fastdfs的tracker服务                               3                    [OK]
manuku/fastdfs-fastdht         fastdfs fastdht                                 2                    [OK]
perfree/fastdfsweb             go-fastdfs文件系统的web管理系统                          2                    
imlzw/fastdfs-storage-dht      fastdfs的storage服务,并且集成了fastdht的服务…              2                    [OK]
leaon/fastdfs                  fastdfs                                         1                    
manuku/fastdfs-tracker         fastdfs tracker                                 1                    [OK]
appcrash/fastdfs_nginx         fastdfs with nginx                              1                    
basemall/fastdfs-nginx         fastdfs with nginx                              1                    [OK]
lionheart/fastdfs_tracker      fastdfs file system‘s tracker node              1                    
tsl0922/fastdfs                FastDFS is an open source high performance d…   0                    [OK]
manuku/fastdfs-storage-dht     fastdfs storage dht                             0                    [OK]
manuku/fastdfs-storage-proxy   fastdfs storage proxy                           0                    [OK]
germicide/fastdfs              The image provides  pptx\docx\xlsx to pdf,mp…   0                    
mypjb/fastdfs                  this is a fastdfs docker project                0                    [OK]
chenfengwei/fastdfs                                                            0                    
```

选择 delron/fastdfs

```
# 下载镜像
docker pull delron/fastdfs
```

### 创建出所需要的目录

我们先把需要的一些目录创建出来(数据目录、数据存储目录等)，执行命令：

```bash
mkdir -p /home/feelapple/fastdfs/tracker/data
mkdir -p /home/feelapple/fastdfs/storage/data
mkdir -p /home/feelapple/fastdfs/storage/path
mkdir -p /home/feelapple/fastdfs/nginx/
```

### 创建`tracker`容器（跟踪服务器容器）

```bash
docker run -dti --network=host --restart=always --name tracker -v /home/feelapple/fastdfs/tracker:/var/fdfs
-v /etc/localtime:/etc/localtime delron/fastdfs tracker

这个命令是用于在 Docker 中运行 FastDFS 的 Tracker 容器。下面是各个参数的解释：

- `-dti`：在后台运行容器，并分配一个伪终端。
- `--network=host`：使用主机网络模式，容器与宿主机共享网络命名空间。
- `--restart=always`：设置容器在退出时总是重启。
- `--name tracker`：给容器指定一个名称（"tracker"）。
- `-v /home/feelapple/fastdfs/tracker:/var/fdfs`：将宿主机的 `/home/feelapple/fastdfs/tracker` 目录映射到容器中的 `/var/fdfs` 目录。这个参数用于持久化保存 Tracker 的数据。
- `-v /etc/localtime:/etc/localtime`：将宿主机的当前时区设置映射到容器中，保持容器和宿主机时区一致。
- `delron/fastdfs`：指定使用的 FastDFS 镜像。
- `tracker`：指定容器运行的命令（这里是启动 Tracker）。
```

### 创建`storage`容器（存储服务器容器）

执行命令（非最终执行命令，请修改为自己的ip地址）：

```bash
docker run -dti --network=host --restart=always --name storage -e TRACKER_SERVER=192.168.1.109:22122 -v /home/feelapple/fastdfs/storage:/var/fdfs -v /etc/localtime:/etc/localtime delron/fastdfs storage

这个命令是用于在 Docker 中运行 FastDFS 的 Storage 容器。下面是各个参数的解释：

- `-dti`：在后台运行容器，并分配一个伪终端。
- `--network=host`：使用主机网络模式，容器与宿主机共享网络命名空间。
- `--restart=always`：设置容器在退出时总是重启。
- `--name storage`：给容器指定一个名称（"storage"）。
- `-e TRACKER_SERVER=192.168.1.109:22122`：设置环境变量 `TRACKER_SERVER`，指定 Tracker 服务器的 IP 地址和端口。
- `-v /home/feelapple/fastdfs/storage:/var/fdfs`：将宿主机的 `/home/feelapple/fastdfs/storage` 目录映射到容器中的 `/var/fdfs` 目录。这个参数用于持久化保存 Storage 的数据。
- `-v /etc/localtime:/etc/localtime`：将宿主机的当前时区设置映射到容器中，保持容器和宿主机时区一致。
- `delron/fastdfs`：指定使用的 FastDFS 镜像。
- `storage`：指定容器运行的命令（这里是启动 Storage）。
```

默认情况下在Storage服务中是帮我们安装了Nginx服务的，相关的端口为：

- 服务 默认端口
  tracker 22122
  storage 23000
  Nginx 8888

### 配置文件的查看&根据要求自行修改(比如端口冲突)

注意：如果要修改端口或者端口冲突了，下面这俩个配置文件都要修改。

```
[root@VM-4-9-centos ~]# docker exec -it storage /bin/bash
[root@VM-4-9-centos nginx-1.12.2]# ls
CHANGES  CHANGES.ru  LICENSE  Makefile  README  auto  conf  configure  contrib  html  man  objs  src
[root@VM-4-9-centos nginx-1.12.2]# cd /    
[root@VM-4-9-centos /]# ls
anaconda-post.log  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@VM-4-9-centos /]# cd etc/fdfs/
[root@VM-4-9-centos fdfs]# ls
client.conf         http.conf   mod_fastdfs.conf  storage.conf.sample  storage_ids.conf.sample  tracker.conf.sample
client.conf.sample  mime.types  storage.conf      storage_ids.conf     tracker.conf
[root@VM-4-9-centos fdfs]# cat storage.conf
```

```
[root@VM-4-9-centos fdfs]# cd /usr/local/nginx  
[root@VM-4-9-centos nginx]# ll
total 36
drwx------ 2 nobody root 4096 Oct 25 14:47 client_body_temp
drwxr-xr-x 1 root   root 4096 Apr 29  2018 conf
drwx------ 2 nobody root 4096 Oct 25 14:47 fastcgi_temp
drwxr-xr-x 2 root   root 4096 Apr 29  2018 html
drwxr-xr-x 1 root   root 4096 Oct 25 14:47 logs
drwx------ 2 nobody root 4096 Oct 25 14:47 proxy_temp
drwxr-xr-x 2 root   root 4096 Apr 29  2018 sbin
drwx------ 2 nobody root 4096 Oct 25 14:47 scgi_temp
drwx------ 2 nobody root 4096 Oct 25 14:47 uwsgi_temp
[root@VM-4-9-centos nginx]# cd conf/
[root@VM-4-9-centos conf]# ls
fastcgi.conf          fastcgi_params          koi-utf  mime.types          nginx.conf          scgi_params          uwsgi_params          win-utf
fastcgi.conf.default  fastcgi_params.default  koi-win  mime.types.default  nginx.conf.default  scgi_params.default  uwsgi_params.default
[root@VM-4-9-centos conf]# cat nginx.conf
```

## 文件上传测试



### 首先在虚拟机的/home/feelapple/fastdfs/storage下保存一张图片,通俗xftp直接上传

### 执行命令，进入 `tracker` 容器中：

```bash
root@feelapple-virtual-machine:~# docker exec -it storage bash
[root@feelapple-virtual-machine nginx-1.12.2]# cd /var/fdfs/
[root@feelapple-virtual-machine fdfs]# ls
1.jpg  2.webp  data  logs  path
[root@feelapple-virtual-machine fdfs]# /usr/bin/fdfs_upload_file /etc/fdfs/client.conf 1.jpg
group1/M00/00/00/wKgBbWTCkeeAItWgACb-OLJuFeM463.jpg
```

- 上传文件的指令
  /usr/bin/fdfs_upload_file /etc/fdfs/client.conf 1.jpg
- 上传成功后根据返回的地址在浏览器中进行访问
- 返回的路径
  group1/M00/00/00/wKgBbWTCkeeAItWgACb-OLJuFeM463.jpg

### 浏览器地址栏中输入地址，直接访问

http://192.168.1.109:8888/group1/M00/00/00/wKgBbWTCkeeAItWgACb-OLJuFeM463.jpg

成功访问:

![](../images/fastDFS/访问图片.png)

## 配置Nginx

前面的补充已经提到了，默认上传的文件是只能在本机访问的，当然这样肯定是不行的，所以我们需要配置一下Nginx 来帮我们实现 Web 访问的效果。

创建nginx目录：

```
docker exec -it storage bash
cd /usr/local/nginx  
cd conf/
vi nginx.conf
```

```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    
    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    server {
        listen       8888;
        listen [::]:8888;
        server_name  localhost;
        location ~/group[0-9]/ {
            ngx_fastdfs_module;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root html;
        }
    }

    
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

### 设置tracker和storage开启自启动

```bash
docker update --restart=always tracker
docker update --restart=always storage
```