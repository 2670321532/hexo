---
title: Ubuntu防火墙放行指定端口
date: 2023/7/8 19:46:25
comments: true
tags: 
- Ubuntu
- 防火墙
- 端口
categories: 
- 计算机
- Ubuntu
---

# Ubuntu  UFW防火墙放行指定端口教程

| enable                 | 启动ufw               |
| ---------------------- | --------------------- |
| disable                | 关闭ufw               |
| reload                 | 重新加载ufw           |
| logging on\|off\|LEVEL | 日志 启动\|关闭\|级别 |
| reset                  | 重置配置              |
| status                 | 查看状态              |
| show REPORT            | 显示“报告”            |

在Ubuntu 系统中，UFW是一种简单的防火墙服务，可以帮助你保护计算机免受恶意攻击。它可以轻松地配置规则，以允许或阻止特定的IP地址、端口和协议通过网络访问服务器。

在本文中，我们将学习如何在Ubuntu 20.04中使用UFW开放指定的TCP端口。

### **第一步：安装UFW**

一般来说，Ubuntu 20.04系统默认已经安装好UFW防火墙，如果你的Ubuntu 20.04系统尚未安装UFW，则需要先进行安装。可以使用以下命令进行安装：

```text
sudo apt update
sudo apt install ufw
```

### **第二步：查看UFW状态**

在使用UFW之前，你需要检查一下它的状态。使用以下命令可以检查UFW是否处于活动状态：

```text
sudo ufw status
```

如果输出结果中显示的状态是`inactive`，则表示UFW当前未开启。

### **第三步：开启UFW**

如果你发现UFW未启用，则需要启用防火墙。使用以下命令可以启用UFW：

```text
sudo ufw enable
```

执行该命令后，UFW将会自动运行，并在系统启动时自动启动。

可以通过执行以下命令来验证UFW的状态：

```text
sudo ufw status
```

如果看到输出结果中的状态显示为 `active`，则表示UFW正在运行。

### **第四步：放行指定端口**

现在，你可以开放指定的TCP端口。使用以下命令可以开放一个指定的TCP端口：

```text
sudo ufw allow <端口号>/tcp
```

例如，要开放80端口：

```text
sudo ufw allow 80/tcp
```

执行该命令后，你会看到类似以下的输出，表示成功添加规则：

```text
Rule added
Rule added (v6)
```

### **第五步：删除指定的端口**

如果你需要删除之前添加的开放端口规则，可以使用以下命令：

```text
sudo ufw delete allow <端口号>/tcp
```

例如，如果要删除之前添加的80端口规则，可以使用以下命令：

```text
sudo ufw delete allow 80/tcp
```

执行命令后，系统会提示你是否确认要删除该规则，如果确认，请选择 `y`，否则选择 `n`

### **第六步：查看UFW规则**

可以使用以下命令来查看UFW当前的规则（已开放的所有端口）：

```text
sudo ufw status verbose
```

该命令将显示当前所有的UFW规则，包括默认规则和已添加的规则。

在Ubuntu 20.04中使用UFW开放指定的TCP端口非常简单。只需遵循本文中的上述步骤，你就可以在你的服务器上轻松配置UFW规则了。