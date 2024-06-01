---
title: CentOS使用
date: 2023/7/8 19:46:25
comments: true
tags: 
- CentOS
- 系统
categories: 
- 计算机
- CentOS
---



# 下载

http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1511.iso

# 设置网络

![](/images/CentOS/1.png)

1. 找到etc/sysconfig/network-scripts 目录下的文件夹eno16777736

```
cd /etc/sysconfig/network-scripts
ll
SET PASSWORD = PASSWORD("Dwz110...");
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Dwz110...' WITH GRANT OPTION;
```

2. 打开文件修改.将ONBOOT=no改成ONBOOT=yes，然后进行文件保存

```
vi ifcfg-eno16777736
```

![](/images/CentOS/2.png)

3、修改完文件之后需要重启下网络服务，IP地址的修改才能生效，再次使用ifconfig命令可以看到已有ip地址

```
systemctl restart network
```

![](/images/CentOS/3.png)

# 安装MySql

```
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
yum -y localinstall mysql57-community-release-el7-11.noarch.rpm
yum repolist enabled | grep "mysql.*-community.*"
yum -y install mysql-community-server --nogpgcheck
systemctl start mysqld.service
ps -ef | grep mysql
systemctl enable mysqld.service
[root@weikeu ~]# grep 'temporary password' /var/log/mysqld.log
2019-06-04T11:59:01.477099Z 1 [Note] A temporary password is generated for root@localhost: P0Deait,dWr6


[root@weikeu ~]# mysql -p'P0Deait,dWr6'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.26
 
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql>

mysql> SET PASSWORD = PASSWORD("AWas@4@5##ji5");
Query OK, 0 rows affected, 1 warning (0.00 sec)
 
mysql> ALTER USER  'root'@'localhost' PASSWORD EXPIRE NEVER;
Query OK, 0 rows affected (0.00 sec)
 
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
 
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'AWas@4@5##ji5' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

## Mysql8安装及问题总结

```
Mysql8安装成功后，首次进入mysql
cat mysqld.log | grep password
找到临时密码
mysql -uroot -p 输入临时密码登录成功
输入mysql语句报错 ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
解决方法 alter user 'root'@'localhost' identified by '你的密码包含大小写及特殊符号8位以上';
flush privileges;
\q退出
使用新密码，重新登录mysql -uroot -p
允许root用户远程访问，则要修改权限

1.登录数据库
mysql -uroot -p 回车
输入密码… 回车

2.登录成功后，切换数据库
use mysql;

3.查看当前用户
select user,host from user;

4.允许root用户远程访问
update user set host='%' where user='root';
GRANT ALL ON . TO 'root'@'%';
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的数据库密码';

5.Navicat远程连接数据库成功
建站时无法连接数据库，报错The server requested authentication method unknown to the client
这是因为mysql8 验证方式与之前的版本不同
在my.cnf中加一句：
default_authentication_plugin=mysql_native_password
变成原来的验证方式即可
```

这种安装方式是压缩包直接解压安装，需要配置连接，安装依赖包，不如yum安装方便。仅供学习参考 。

## 一.准备环境

### 1.缷载maridb

一般centos都会预装maridb,这个可能会与mysql冲突,先卸载它

```
# 查看是否自带mariadb数据库
[root@node2 ~]# rpm -qa|grep mariadb
mariadb-libs-5.5.68-1.el7.x86_64

# 卸载mariadb
[root@node2 ~]# rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64

# 检查卸载结果,如果输出为空,表示卸载完毕
[root@node2 ~]# rpm -qa|grep mariadb
```

### 2.安装wget

wget是一款好用的下载工具,一会下载软件包会用到.

```
# 查找wget
# 如果有内容,表示还未安装
[root@node2 ~]# rpm -qa|grep wget  

# 安装wget  
[root@node2 ~]# yum -y install wget
```

### 3.安装依赖项

```
# 查找libaio
[root@node2 ~]# rpm -qa|grep libaio
# 安装libaio
[root@node2 ~]# yum -y install libaio

# 检查numactl  
[root@node2 ~]# rpm -qa|grep numactl  

# 安装numactl  
[root@node2 ~]# yum -y install numactl
```

## 二.下载并解压安装包

### 1.使用wget工具下载mysql安装包

```
[root@node2 ~]# cd ~ && wget https://cdn.mysql.com/Downloads/MySQL-8.0/mysql-8.0.30-el7-x86_64.tar
```

### 2.解压文件

```
# 把刚才下载的文件解压到 /usr/local目录下

[root@node2 ~]# tar -xvf mysql-8.0.30-el7-x86_64.tar -C /usr/local
mysql-test-8.0.30-el7-x86_64.tar.gz
mysql-8.0.30-el7-x86_64.tar.gz
mysql-router-8.0.30-el7-x86_64.tar.gz

# 可以看到,解压出了三个.gz文件,
# 进入/usr/local,继续解压
[root@node2 ~]# cd /usr/local && tar -xzvf mysql-8.0.30-el7-x86_64.tar.gz

# 把解压出的目录,重命名为 mysql
[root@node2 local]# mv mysql-8.0.30-el7-x86_64 mysql
```

## 三.用户权限

创建一个命名为"mysql"的用户来管理数据库相关的文件

```
# 创建用户组
[root@node2 local]# groupadd mysql

# 创建用户
[root@node2 local]# useradd -g mysql mysql

# 更改mysql目录的所属
[root@node2 local]# chown -R mysql:mysql /usr/local/mysql
```

## 四.配置文件

### 1 创建配置文件

```
[root@node2 local]# vi /etc/my.cnf
```

配置文件可以依据你自己的实际情况配置,

如果不熟悉可以复制下面这个

```
# 客户端配置
[client]
port = 3306
socket = /usr/local/mysql/mysql.sock
default-character-set = utf8mb4

# 服务端配置
[mysqld]
user = mysql
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data

# log-error = /usr/local/mysql/error.log
# pid-file = /usr/local/mysql/mysqld.pid

port = 3306
socket = /usr/local/mysql/mysql.sock

character-set-server = utf8mb4
collation-server = utf8mb4_general_ci
init_connect = 'SET NAMES utf8mb4'

# 表名是否区分大小,默认是0,表区分大小写;1代表不区分大小写，以小写存储
lower_case_table_names = 1
```

### 2 把配置文件授权给mysql用户

```
[root@node2 local]# chown mysql:mysql /etc/my.cnf
```

## 五,mysql初始化并启动

### 1.初始化

```
[root@node2 local]# /usr/local/mysql/bin/mysqld --initialize
2023-05-25T08:22:09.454308Z 0 [System] [MY-013169] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.30) initializing of server in progress as process 22429
2023-05-25T08:22:09.469037Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2023-05-25T08:22:10.485336Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2023-05-25T08:22:11.551887Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: bhH<quyrq3NY
```

从打印出的日志可以看到,mysql自动生成了一个初始密码 bhH<quyrq3NY ,就在日志最后面.把这个密码记下来,一会登录要用.

### 2.启动数据库

```
[root@node2 ~]# /usr/local/mysql/support-files/mysql.server start
Starting MySQL.. SUCCESS!
```


好了你的数据库服务就已经启动了.不过想操作数据,还需要一个客户端.mysql自带的客户端就在/usr/local/mysql/bin/目录下

### 3.启动客户端,连接数据库

```
[root@node2 ~]# /usr/local/mysql/bin/mysql -p
Enter password: 
```


要求你输入密码,把刚才那个初始密码输上,然后敲回车

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.30 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

看到上边这个界面,就表示已经连接成功了.是不是想大展拳脚了,不过,还不要着急,第一次使用,mysql会要求你修改下初始密码

### 4.修改密码

```
mysql> set password for root@localhost = '123';
Query OK, 0 rows affected (0.00 sec)
刚开始,为了练习方便,可以设置一个好记的.这里设置的是123.
```

### 5.允许远程登录

如果你想远程连接mysql,还可以在mysql里设置下

```
mysql> use mysql;
mysql> update user set user.Host='%' where user.User='root';
mysql> flush privileges;
```

好了,现在大功告成了. 开始使用它吧.

## 六.优化启动指令

如果你嫌启动指令有点复杂. 或者想设置开机自启动,可以进一步优化.

### 1.优化客户端启动指令

实现原理,就是在系/usr/bin 目录下,建立一个客户端启动程序的软连接.类似于在window系统下,配置环境变量

```
# 软连接

[root@node2 local]# ln -s /usr/local/mysql/bin/mysql /usr/bin
```


以后可以直接用mysql启动客户端了

```
mysql -uroot -p
```

### 2.优化服务器端启动指令

同理,把服务端的启动程序,软连接到系统启动目录下:

```
[root@node2 ~]# ln -s /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
```

以后可以用下列指令操控mysql服务

```
#开启服务
service mysql start

#关闭服务
service mysql stop

#重启服务
service mysql restart

#查看服务状态
service mysql status
```

### 3.配置开机自启动

如果需要服务器重启时,mysql能够自动运行,可以把mysql添加到自启列表

```
# 添加到自启动列表

[root@node2 local]# chkconfig --add mysqld

# 可以重启机器,验证下

[root@node2 local]# reboot

# 查看mysql是否运行

[root@node2 local]# ps -e|grep mysql
  1176 ?        00:00:00 mysqld_safe
  1635 ?        00:00:00 mysqld
```

centos7以上的系统, 添加到自启列表的服务,还可以用systemctl 命令来管理

```
# 开启服务
[root@node1 ~]# systemctl start mysql

# 关闭服务
[root@node1 ~]# systemctl stop mysql

# 重启服务
[root@node1 ~]# systemctl restart mysql

# 查看服务状态
[root@node1 ~]# systemctl status mysql
```

# 安装Redis

## 一、安装gcc依赖

由于 redis 是用 C 语言开发，安装之前必先确认是否安装 gcc 环境（gcc -v），如果没有安装，执行以下命令进行安装

```
[root@localhost local]# yum install -y gcc 
```

## 二、下载并解压安装包

```
[root@localhost local]# wget http://download.redis.io/releases/redis-6.2.5.tar.gz

[root@localhost local]# tar -zxvf redis-6.2.5.tar.gz
```

## 三、cd切换到redis解压目录下，执行编译

```
[root@localhost local]# cd redis-6.2.5

[root@localhost redis-6.2.5]# make
```

## 四、安装并指定安装目录

```
[root@localhost redis-6.2.5]# make install PREFIX=/usr/local/redis
```

## 五、启动服务

5.1前台启动

```
[root@localhost redis-6.2.5]# cd /usr/local/redis/bin/

[root@localhost bin]# ./redis-server
```

5.2后台启动
从 redis 的源码目录中复制 redis.conf 到 redis 的安装目录

```
[root@localhost bin]# cp /usr/local/redis-6.2.5/redis.conf /usr/local/redis/bin/
```

修改 redis.conf 文件，把 daemonize no 改为 daemonize yes

```
[root@localhost bin]# vi redis.conf
```

![](/images/CentOS/4.png)

后台启动

```csharp
[root@localhost bin]# ./redis-server redis.conf
```

![](/images/CentOS/5.png)

## 六、设置开机启动

添加开机启动服务

```
[root@localhost bin]# vi /etc/systemd/system/redis.service
```

复制粘贴以下内容：

```
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/bin/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

注意：ExecStart配置成自己的路径 

设置开机启动

```
[root@localhost bin]# systemctl daemon-reload

[root@localhost bin]# systemctl start redis.service

[root@localhost bin]# systemctl enable redis.service
```

创建 redis 命令软链接

```
[root@localhost ~]# ln -s /usr/local/redis/bin/redis-cli /usr/bin/redis
```

测试 redis

![](/images/CentOS/6.png)

服务操作命令

```
systemctl start redis.service   #启动redis服务

systemctl stop redis.service   #停止redis服务

systemctl restart redis.service   #重新启动服务

systemctl status redis.service   #查看服务当前状态

systemctl enable redis.service   #设置开机自启动

systemctl disable redis.service   #停止开机自启动
```

## 七、开放外部访问

配置 vim /etc/redis.conf

![](/images/CentOS/7.png)

2 查看防火墙状态

```sh
yum -y install firewalld   # 下载firewalld

# 1、启动FirewallD服务命令：
systemctl start firewalld.service #开启服务
systemctl enable firewalld.service #设置开机启动
# 2、查看FirewallD防火墙状态：
systemctl status firewalld
# 开启某个端口（6379）      
firewall-cmd --zone=public --add-port=6379/tcp --permanent
```

重启防火墙

```sql
firewall-cmd --reload
```

3、现在防火墙 FirewallD 就已经正常运行了。

```sh
FirewallD 常用的命令：
firewall-cmd --state ##查看防火墙状态，是否是running
systemctl status firewalld.service ##查看防火墙状态
systemctl start firewalld.service ##启动防火墙
systemctl stop firewalld.service ##临时关闭防火墙
systemctl enable firewalld.service ##设置开机启动防火墙
systemctl disable firewalld.service ##设置禁止开机启动防火墙

firewall-cmd --permanent --query-port=80/tcp ##查看80端口有没开放
firewall-cmd --reload ##重新载入配置，比如添加规则之后，需要执行此命令
firewall-cmd --get-zones ##列出支持的zone
firewall-cmd --get-services ##列出预定义的服务
firewall-cmd --query-service ftp ##查看ftp服务是否放行，返回yes或者no
firewall-cmd --add-service=ftp ##临时开放ftp服务
firewall-cmd --add-service=ftp --permanent ##永久开放ftp服务
firewall-cmd --remove-service=ftp --permanent ##永久移除ftp服务
firewall-cmd --add-port=80/tcp --permanent ##永久添加80端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent ##移除80端口
iptables -L -n ##查看规则，这个命令是和iptables的相同的
man firewall-cmd ##查看帮助

参数含义：
--zone #作用域
--permanent #永久生效，没有此参数重启后失效
```



# tftp server安装配置 (fileZilla工具下载)

1、命令安装 tftp server

```
[root@localhost ~]# yum -y install tftp
[root@localhost ~]# yum -y install tftp-server
```

 2、配置 tftp 的配置文件vi /etc/xinetd.d/tftp  

     # 将修改 disable  的值为  no
     [root@localhost ~]#vi /etc/xinetd.d/tftp

```
# default: off
# description: The tftp server serves files using the trivial file transfer \
#       protocol.  The tftp protocol is often used to boot diskless \
#       workstations, download configuration files to network-aware printers, \
#       and to start the installation process for some operating systems.

service tftp
{
        socket_type             = dgram
        protocol                = udp
        wait                    = yes
        user                    = root
        server                  = /usr/sbin/in.tftpd
        server_args             = -s /var/lib/tftpboot
        disable                 = no
        per_source              = 11
        cps                     = 100 2
        flags                   = IPv4
}
```


 3、开启 xinetd.service

```
[root@localhost ~]#systemctl restart xinetd.service
```

 注：xinetd的安装：

```
[root@localhost init.d]# yum -y install xinetd 
[root@localhost xinetd.d]# rpm -qa | grep xinetd  
xinetd-2.3.15-12.el7.x86_64  
```

 4、查看tftp server开启状态

查看到69端口启动了说明tftp server已经启动成功并正常运行

```
[root@localhost lib]# netstat -a | grep tftp
udp        0      0 0.0.0.0:tftp            0.0.0.0:*                          
[root@localhost lib]# netstat -tunap | grep  :69
udp        0      0 0.0.0.0:69              0.0.0.0:*                           958/xinetd    
```

5、使用fileZilla（window客户端）连接tftp服务

# 安装Python

去官网下载Python，上传到服务器
来到官网 https://www.python.org/ftp/python/3.8.6/，选择下面这个文件进行下载，如果想要下载其他版本的可以另行选择，最后下载的也需要是tgz结尾的。

- 解压压缩文件
  可以看到这里已经上传好了，接下来就是解压
  输入命令：

  ```
  tar -zxvf Python-3.8.6.tgz
  ```

  

- 安装Python依赖包
  这里需要使用yum进行安装，前提是更换好了yum源。
  [更换为清华源](https://blog.csdn.net/weixin_59269336/article/details/126288842)
  可以使用以下命令安装Python的依赖包

  ```
  yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make libffi-devel -y
  ```

- 进行预编译
  进入到解压后的Python文件夹当中输入下面的命令

  ```
  ./configure prefix=/usr/local/python3
  ```

  注意：
  prefix后面跟的是Python的安装地址，可以自行选定。
  预编译之后没有报错（如下图），就可以进行下一步

![](/images/CentOS/8.png)

- 进行安装
  输入命令：

```
make && make install
```

这个可能需要一点时间，安装成功之后没有报错就可以使用Python了。
出现一下提示即表示安装成功

![](/images/CentOS/9.png)

输入python进行验证。

[Centos7下安装Elasticsearch 5.6.6](https://www.cnblogs.com/luxiaojun/p/11848663.html)

## 环境

因为elasticsearch是用java编写的，所以需要先安装JDK

ES 5，安装需要 JDK 8 以上
ES 6.5，安装需要 JDK 11 以上
ES 7.2.1，内置了 JDK 12

## 安装jdk11

```
yum -y install java-11-openjdk.x86_64
```

## 使用 **wget** 命令下载

```
wget https:``//artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.6.tar.gz
```

# 安装elasticsearch

1、解压elasticsearch到 /usr/local 目录下

```
tar -zxvf elasticsearch-5.6.6.tar.gz -C /usr/local/
```

2、elasticsearch默认是不支持用root用户来启动的，需要添加专门的用户

```
cd /usr/local``useradd elastic``chown -R elastic:elastic elasticsearch-5.6.6/``su elastic``./elasticsearch-5.6.6/bin/elasticsearch -d
```

2、进入到解压的elasticsearch包目录下、启动参数 -d 指的是后台运行

```
cd /usr/local/elasticsearch-5.6.6/``./bin/elasticsearch -d
```

3、使用  curl http://localhost:9200/  查看是否运行，如果返回如下信息则标示运行正常

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142211853-2043812384.png)

4、elasticsearch默认restful-api的端口是9200 不支持Ip地址，只能在本机用http://localhost:9200来访问。如果需要改变，需要修改配置文件。

默认情况下 Elasticsearch 的 RESTful 服务只有本机才能访问，也就是说无法从主机访问虚拟机中的服务。

可以修改 /etc/elasticsearch/config/elasticsearch.yml 文件，将注释的 network.host 和 http.port 放开，并配置正确的IP（或者0.0.0.0）

```
cd /usr/local/elasticsearch-5.6.6``vim config/elasticsearch.yml
```

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142354572-2045886615.png)

5、先将Elasticsearch 关闭，然后启动；

关闭方法：输入命令： ps -ef | grep elasticsearch ，找到进程，然后kill掉就行了；

启动方法：输入命令：su elastic ， 然后输入 ./bin/elasticserach -d 

6、在谷歌浏览器中打开：http://{server_IP}:9200/

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142529552-1658449797.png)

## 问题

**1****、问题一**：启动elasticsearch报错如下：Java HotSpot(TM) 64-Bit Server VM warning: INFO: os::commit_memory(0x0000000085330000, 2060255232, 0) failed; error='Cannot allocate memory' (errno=12)

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142613581-1470381087.png)

 **解决方法：**

   由于elasticsearch5.0默认分配jvm空间大小为2g，修改jvm空间分配： vim config/jvm.options

```
-Xms2g
-Xmx2g
```

  修改为

```
-Xms512m
-Xmx512m
```

 **2、问题二：**启动elasticsearch报错如下：org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142710947-1828234782.png)

**解决方法：**

原因是elasticsearch默认是不支持用root用户来启动的，需要添加专门的用户。

```
cd /usr/local
useradd elastic
chown -R elastic:elastic elasticsearch-5.6.6/
su elastic
./elasticsearch-5.6.6/bin/elasticsearch -d
```

**3、问题三**：启动报错如下

ERROR: [3] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [1775] for user [elastic] is too low, increase to at least [2048]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

**解决方法：**

（1）[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [**65536**]

　　  [2]: max number of threads [1775] for user [elastic] is too low, increase to at least [**2048**]

先切换到root账户下面，使用 **vi /etc/security/limits.conf** ，增加如下内容

```
elastic soft nofile 65536``elastic hard nofile 65536``elastic soft nproc 2048``elastic hard nproc 2048
```

（2）[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

使用 **vim /etc/sysctl.conf** ，增加如下的内容

```
vm.max_map_count=262144
```

输入：**sysctl -p** ，如下所示

![img](https://img2018.cnblogs.com/blog/940167/201911/940167-20191113142845654-1683151802.png)

（3）重新启动elasticsearch

```
su elastic``cd /usr/local/elasticsearch-5.6.6``./bin/elasticsearch -d
```



# CentOS开关机

关机:sudo shutdown now

重启:reboot