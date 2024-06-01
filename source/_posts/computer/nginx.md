---
title: nginx
date: 2013/7/13 20:46:25
comments: true
tags: nginx
categories: 
- 计算机
- nginx
---

## 一、安装nginx之前，安装一下工具

```
sudo apt update
sudo apt-get install libpcre3-dev
sudo apt-get install ruby
sudo apt-get install zlib1g-dev
```

## 安装Nginx

```
wget http://nginx.org/download/nginx-1.24.0.tar.gz
sudo -i 
tar -zxvf nginx-版本号.tar.gz

cd nginx-版本号/


#编译
./configure  --with-http_ssl_module

./configure 

#提示没有安装的工具包，见步骤一，并且删除nginx-1.22.1，重新安装

#安装
make && make install
```

### Nginx配置：

默认安装到/usr/local/nginx/目录，进入目录。

启动nginx：

```
#启动
sudo sbin/nginx
#查看
ps aux | grep nginx
#停止
sudo sbin/nginx -s stop
```

打开/usr/local/nginx/conf/nginx.conf文件


​    
​    server {
​        # 监听80端口
​        listen 80;
​        # ipv6
​        listen [::]:80;
​        # 本机
​        server_name localhost; 
​        # 默认请求的url
​        location / {
​            #请求转发到gunicorn服务器
​            proxy_pass http://127.0.0.1:5001; 
​            #设置请求头，并将头信息传递给服务器端 
​            proxy_set_header Host $host; 
​        }
​    }


​    
​    
​    worker_processes  1;
​    
    #error_log  logs/error.log;
    #error_log  logs/error.log  notice;
    #error_log  logs/error.log  info;
    
    #pid        logs/nginx.pid;


​    
​    events {
​        worker_connections  1024;
​    }


​    
​    http {
​        include       mime.types;
​        default_type  application/octet-stream;
​    
    upstream flask{
    		# 添加服务器负载均衡
            server 192.168.1.109:6000;
            server 127.0.0.1:5000;
        }
    server {
            listen       80;
            server_name  localhost;
    
            #charset koi8-r;
    
            #access_log  logs/host.access.log  main;
    
            location / {
                proxy_pass http://flask;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
            }
    
            #error_page  404              /404.html;
    
            # redirect server error pages to the static page /50x.html
            #
            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
                root   html;
            }
    }
