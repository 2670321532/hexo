---
title: ubuntu安装常用软件
date: 2023/7/8 10:46:25
comments: true
tags: 
- ubuntu
categories: 
- 计算机
- ubuntu
---

# 下载谷歌浏览器（chrome)

打开该网站https://www.chromedownloads.net/chrome64linux-stable/

找到自己合适的谷歌浏览器版本后下载

打开中终端输入解压命令来解压文件

```
sudo dpkg -i  google-chrome-stable_current_amd64.deb
```

如果在解压的时候发生错误可以在终端尝试输入

```
sudo apt-get -f install
```

安装成功后在次尝试解压文件

使用命令行进行下载：

对于谷歌Chrome32位版本，使用如下链接：

```
Wget https://dl.google.com/linux/direct/google-chrome-stable_current_i386.deb
```

以下为64位版本，使用下面的命令。

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

下载完后，运行如下命令安装。

```
sudo dpkg -i google-chrome*

sudo apt-get -f install
```

# 安装pycharm

## 1、下载

下载地址：[https://www.jetbrains.com/pycharm/download/#section=linux](https://blog.csdn.net/qq_44920726/article/details/123504135#section=linux)

## 2、解压

安装包将被下载到Downloads文件夹下，选择***\**\*安装包\*\**\***右键点击***\**\*提取到此处\*\**\***进行解压。

## 3、安装

在终端进入/pycharm-community-2017.3.3/bin目录下，执行：

```
cd Downloads/pycharm/bin
sh ./pycharm.sh
```

## 4、破解使用

打开script文件夹,打开终端,使用命令

看到下图框选的两个脚本，

- install.sh：是用于Mac和Linux系统的激活脚本。
- install-all-users.vbs：是用于Windows系统的激活脚本。

```
sh ./install.sh
```

然后我们打开 pycharm，如下图所示点击 **Activation code** 。

```
EUWT4EE9X2-eyJsaWNlbnNlSWQiOiJFVVdUNEVFOVgyIiwibGljZW5zZWVOYW1lIjoic2lnbnVwIHNjb290ZXIiLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiIiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJQU0kiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBDIiwiZmFsbGJhY2tEYXRlIjoiMjAyNS0wOC0wMSIsInBhaWRVcFRvIjoiMjAyNS0wOC0wMSIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiUFBDIiwiZmFsbGJhY2tEYXRlIjoiMjAyNS0wOC0wMSIsInBhaWRVcFRvIjoiMjAyNS0wOC0wMSIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQV1MiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBDV01QIiwiZmFsbGJhY2tEYXRlIjoiMjAyNS0wOC0wMSIsInBhaWRVcFRvIjoiMjAyNS0wOC0wMSIsImV4dGVuZGVkIjp0cnVlfV0sIm1ldGFkYXRhIjoiMDEyMDIyMDkwMlBTQU4wMDAwMDUiLCJoYXNoIjoiVFJJQUw6MzUzOTQ0NTE3IiwiZ3JhY2VQZXJpb2REYXlzIjo3LCJhdXRvUHJvbG9uZ2F0ZWQiOmZhbHNlLCJpc0F1dG9Qcm9sb25nYXRlZCI6ZmFsc2V9-FT9l1nyyF9EyNmlelrLP9rGtugZ6sEs3CkYIKqGgSi608LIamge623nLLjI8f6O4EdbCfjJcPXLxklUe1O/5ASO3JnbPFUBYUEebCWZPgPfIdjw7hfA1PsGUdw1SBvh4BEWCMVVJWVtc9ktE+gQ8ldugYjXs0s34xaWjjfolJn2V4f4lnnCv0pikF7Ig/Bsyd/8bsySBJ54Uy9dkEsBUFJzqYSfR7Z/xsrACGFgq96ZsifnAnnOvfGbRX8Q8IIu0zDbNh7smxOwrz2odmL72UaU51A5YaOcPSXRM9uyqCnSp/ENLzkQa/B9RNO+VA7kCsj3MlJWJp5Sotn5spyV+gA==-MIIETDCCAjSgAwIBAgIBDTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTIwMTAxOTA5MDU1M1oXDTIyMTAyMTA5MDU1M1owHzEdMBsGA1UEAwwUcHJvZDJ5LWZyb20tMjAyMDEwMTkwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCUlaUFc1wf+CfY9wzFWEL2euKQ5nswqb57V8QZG7d7RoR6rwYUIXseTOAFq210oMEe++LCjzKDuqwDfsyhgDNTgZBPAaC4vUU2oy+XR+Fq8nBixWIsH668HeOnRK6RRhsr0rJzRB95aZ3EAPzBuQ2qPaNGm17pAX0Rd6MPRgjp75IWwI9eA6aMEdPQEVN7uyOtM5zSsjoj79Lbu1fjShOnQZuJcsV8tqnayeFkNzv2LTOlofU/Tbx502Ro073gGjoeRzNvrynAP03pL486P3KCAyiNPhDs2z8/COMrxRlZW5mfzo0xsK0dQGNH3UoG/9RVwHG4eS8LFpMTR9oetHZBAgMBAAGjgZkwgZYwCQYDVR0TBAIwADAdBgNVHQ4EFgQUJNoRIpb1hUHAk0foMSNM9MCEAv8wSAYDVR0jBEEwP4AUo562SGdCEjZBvW3gubSgUouX8bOhHKQaMBgxFjAUBgNVBAMMDUpldFByb2ZpbGUgQ0GCCQDSbLGDsoN54TATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBaAwDQYJKoZIhvcNAQELBQADggIBABqRoNGxAQct9dQUFK8xqhiZaYPd30TlmCmSAaGJ0eBpvkVeqA2jGYhAQRqFiAlFC63JKvWvRZO1iRuWCEfUMkdqQ9VQPXziE/BlsOIgrL6RlJfuFcEZ8TK3syIfIGQZNCxYhLLUuet2HE6LJYPQ5c0jH4kDooRpcVZ4rBxNwddpctUO2te9UU5/FjhioZQsPvd92qOTsV+8Cyl2fvNhNKD1Uu9ff5AkVIQn4JU23ozdB/R5oUlebwaTE6WZNBs+TA/qPj+5/we9NH71WRB0hqUoLI2AKKyiPw++FtN4Su1vsdDlrAzDj9ILjpjJKA1ImuVcG329/WTYIKysZ1CWK3zATg9BeCUPAV1pQy8ToXOq+RSYen6winZ2OO93eyHv2Iw5kbn1dqfBw1BuTE29V2FJKicJSu8iEOpfoafwJISXmz1wnnWL3V/0NxTulfWsXugOoLfv0ZIBP1xH9kmf22jjQ2JiHhQZP7ZDsreRrOeIQ/c4yR8IQvMLfC0WKQqrHu5ZzXTH4NO3CwGWSlTY74kE91zXB5mwWAx1jig+UXYc2w4RkVhy0//lOmVya/PEepuuTTI4+UJwC7qbVlh5zfhj8oTNUXgN0AOc+Q0/WFPl1aw5VV/VrO8FCoB15lFVlpKaQ1Yh+DVU8ke+rt9Th0BCHXe0uZOEmH0nOnH/0onD
```

# Ubuntu 安装和使用MySQL

### 更新列表

```
sudo apt-get update
```

### 安装MySQL服务器

```
sudo apt-get install mysql-server
```

在安装过程中，系统将提示您创建root密码。选择一个安全的，并确保记住它，因为后面需要用到这个密码。

### 安装MySQL客户端

```
sudo apt-get install mysql-client
```

mysql-server和mysql-client区别

mysql-server 是MySQL核心程序将安装MySQL数据库服务器，用于生成管理多个数据库实例，持久保存数据并为其提供查询接口（SQL），供不同客户端调用。

mysql-client 是操作数据库实例的工具，允许连接到MySQL服务器使用该查询接口。它将为您提供MySQL命令行程序。

如果只需要连接到远程服务器并运行查询，只安装mysql-client就可以了。如果是服务器只提供连接服务的只需要安装mysql-server

### 配置MySQL

运行MySQL初始化安全脚本

```
sudo mysql_secure_installation
```

mysql_secure_installation脚本设置的东西：更改root密码、移除MySQL的匿名用户、禁止root远程登录、删除test数据库和重新加载权限。除了询问是否要更改root密码时，看情况是否需要更改，其余的问题都可以按Y，然后ENTER接受所有后续问题的默认值。使用上面的这些选项可以提高MySQL的安全。

```
feelapple@feelapple-virtual-machine:~$ sudo mysql_secure_installation

Securing the MySQL server deployment.

Enter password for user root: 

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: n
Using existing password for root.
Change the password for root ? ((Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.
```

### 测试MySQL

无论你如何安装它，MySQL应该已经开始自动运行。要测试它，请检查其状态。

```
systemctl status mysql.service
```

```
feelapple@feelapple-virtual-machine:~$ systemctl status mysql.service
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset:>
     Active: active (running) since Sat 2023-07-08 16:23:04 CST; 14min ago
   Main PID: 3709 (mysqld)
     Status: "Server is operational"
      Tasks: 39 (limit: 19092)
     Memory: 366.4M
        CPU: 2.484s
     CGroup: /system.slice/mysql.service
             └─3709 /usr/sbin/mysqld

7月 08 16:23:04 feelapple-virtual-machine systemd[1]: Starting MySQL Community >
7月 08 16:23:04 feelapple-virtual-machine systemd[1]: Started MySQL Community S>
lines 1-13/13 (END)
```

# 文件拖拽下载

apt install -y lrzsz