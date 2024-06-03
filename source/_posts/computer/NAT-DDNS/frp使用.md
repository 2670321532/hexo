---
title: frp使用
date: 2024/5/30 0:30:25
comments: true
tags: 
- 内网穿透
- nssm
- frp
- 网络
- 端口
- 服务器
categories: 
- 计算机
- DDNS
---
# Frp内网穿透服务

```
1.下载
wget https://github.com/fatedier/frp/releases/download/v0.54.0/frp_0.54.0_linux_amd64.tar.gz
2.解压
tar -zxvf frp_0.54.0_linux_amd64.tar.gz
cd frp_0.54.0_linux_amd64
3.编辑配置文件 frps.toml
bindPort = 7000
webServer.port = 8884 # 面板端口 
webServer.user = 'admin' # 面板登录名
webServer.password = 'dwz110...' # 面板登录密码
webServer.addr = '10.0.4.7'   # 服务器面板访问地址（内网地址 10.0.4.7）
server_domain = "ai.feelapple.cn"
# 可不添加
auth.method = 'token' # 客户端访问验证方式
auth.token = '*6!4k4su046WXs@3uA%7!F8&07&n*bNcK#A#^3pYO^tJriav2K' # 客户端访问验证密码
4. 启动测试
./frp_0.54.0_linux_amd64/frps -c ./frp_0.54.0_linux_amd64/frps.toml # -c 为选择配置文件
5.访问面板
ai.feelapple.cn:8884
```

# 在 Ubuntu 中将 frps 设置为守护进程

可以通过创建一个 Systemd 服务来实现。以下是详细步骤：

根据错误日志信息，`frps.service` 文件中存在两个问题：

1. 使用了 `nobody` 用户，这被认为是不安全的。
2. `ExecStart` 指令的路径配置错误，应该是一个绝对路径，而不是相对路径。

我们需要修改 `frps.service` 文件来解决这些问题。以下是更新后的服务文件：

1. **编辑服务文件**：

   打开 `frps.service` 文件进行编辑：

   ```
   sudo vim /etc/systemd/system/frps.service
   ```
   
2. **更新服务文件内容**：

   将内容更新为以下内容（确保路径正确）：

   ```
   [Unit]
   Description=Frp Server Service
   After=network.target
   
   [Service]
   Type=simple
   User=ubuntu
   Group=ubuntu
   ExecStart=/home/ubuntu/frp_0.54.0_linux_amd64/frps -c /home/ubuntu/frp_0.54.0_linux_amd64/frps.toml
   Restart=on-failure
   
   [Install]
   WantedBy=multi-user.target
   ```

   注意将 `User` 和 `Group` 修改为 `ubuntu` 用户，并确保 `ExecStart` 的路径为 `frps` 可执行文件的绝对路径。

3. **重新加载 Systemd 配置并启动服务**：

   ```
   sudo systemctl daemon-reload
   sudo systemctl start frps
   ```

   ```
   这将重新启动FRPS服务。
   sudo systemctl restart frps
   ```

4. **使服务开机自启动**：

   ```
   sudo systemctl enable frps
   ```

5. **检查服务状态**：

   确认服务状态：

   ```
   sudo systemctl status frps
   ```

通过这些步骤，应该可以解决 `frps` 服务启动失败的问题。如果问题仍然存在，请提供最新的错误日志信息。

# frpc客户端访问设置

1.准备客户端程序（以win10为例）
下载地址：https://github.com/fatedier/frp/releases/download/v0.54.0/frp_0.54.0_windows_amd64.zip

2.更改配置文件 

```
文件frpc.toml
[common]
server_addr = 122.51.202.183
server_port = 7000

[web]
type = tcp 
local_ip = 192.168.1.110
local_port = 8080
remote_port = 8080


[web_movie]
type = tcp 
local_ip = 192.168.1.110
local_port = 8096
remote_port = 8096
```

如果你的 `frpc` 位于 `K:\NWCT\frp` 目录中，我们可以按照以下步骤将其设置为 Windows 服务，并确保它能够正常启动和运行。

### 1. 确保文件路径和配置文件正确

首先，确保 `frpc` 可执行文件和配置文件位于 `K:\NWCT\frp` 目录中。

### 2. 创建 frpc 配置文件

在 `K:\NWCT\frp` 目录下创建一个配置文件 `frpc.ini`。以下是一个基本的配置文件示例：

```
[common]
server_addr = <FRPS_SERVER_IP>
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
```

根据你的实际情况调整配置。

### 3. 创建批处理文件

在 `K:\NWCT\frp` 目录下创建一个批处理文件 `frpc.bat`，内容如下：

```
@echo off
K:\NWCT\frp\frpc.exe -c K:\NWCT\frp\frpc.toml
```

确保路径指向你的 `frpc` 可执行文件和配置文件。

### 4. 使用 NSSM 创建 Windows 服务

#### 下载 NSSM

从 [NSSM 官方网站](https://nssm.cc/) 下载 NSSM，并解压到一个目录，例如 `K:\nssm`.

#### 使用 NSSM 创建服务

打开命令提示符，以管理员身份运行以下命令：

```
cd K:\nssm\win64
nssm install frpc
```

将会弹出一个 NSSM 的配置窗口。

#### 配置 NSSM

在弹出的窗口中，进行以下配置：

- **Path**：`K:\NWCT\frp\frpc.exe`
- **Startup directory**：`K:\NWCT\frp`
- **Arguments**：`-c K:\NWCT\frp\frpc.toml`

#### 配置日志输出（可选）

在 `I/O` 标签下，你可以指定输出日志文件的位置，例如：

- **Output (stdout)**：`K:\NWCT\frp\frpc_output.log`
- **Error (stderr)**：`K:\NWCT\frp\frpc_error.log`

点击 `Install service` 按钮安装服务。

### 5. 启动服务

安装完成后，可以通过命令启动服务：

```
nssm start frpc
```

或者通过服务管理器（services.msc）找到 `frpc` 服务并启动。

### 6. 检查服务状态

在命令提示符中运行以下命令以确认服务状态：

```
nssm status frpc
```

服务应该显示为 `SERVICE_RUNNING`。

### 7. 确保服务开机自启动

默认情况下，NSSM 安装的服务会配置为开机自启动。如果需要确认，可以通过服务管理器（services.msc）检查 `frpc` 服务的启动类型是否为“自动”。

### 8. 验证服务运行情况

查看你配置的日志文件，确保 `frpc` 正常运行：

```
type K:\NWCT\frp\frpc_output.log
type K:\NWCT\frp\frpc_error.log
```

### 9. 查看 Windows 事件查看器

如果服务仍然无法启动，请查看 Windows 事件查看器中的系统日志，以获取更多关于服务启动失败的详细信息：

1. 按 `Win + R` 打开运行窗口，输入 `eventvwr` 并回车。
2. 在左侧导航栏中，展开 `Windows Logs`，然后选择 `System`。
3. 查看与 `frpc` 服务相关的错误或警告日志。