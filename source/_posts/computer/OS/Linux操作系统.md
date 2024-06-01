---
title: Linux操作系统
date: 2023/8/12 11:21:32
comments: true
tags: 
- Linux
- Linux命令
categories: 
- 计算机
- Linux
---

# Linux操作系统

# 学习目标

* Linux系统概述
* Linux系统的安装和体验
* Linux的网络配置和连接工具
* Linux的目录结构
* Linux的常用命令
* Linux的VI编辑器

# 一、操作系统概述

## 1、计算机分类

计算机一般分为个人计算机（笔记、台式机）与 企业级服务器（1U、2U、机柜、塔式、刀片）两种形式。

## 2、计算机组成

计算机资源分为2 部分：==硬件资源、软件资源==

硬件资源：所谓的硬件资源就是看得见、摸得着的

![image-20210906104118846](../images/Linux/media/image-20210906104118846.png)

> 在实际工作中，为软件资源提供硬件保障



软件资源：看得见、摸不着（如QQ、Wechat、WPS）

思考问题：操作硬件，软件有响应。操作软件，硬件也有响应。

![image-20210906105138366](../images/Linux/media/image-20210906105138366.png)

思考：软件可以操作硬件（听音乐）、硬件也可以操作软件（玩游戏，人物的移动），它们之间是如何交互的呢？

答：主要就是==由于操作系统==，可以这么理解操作系统是软硬件之间的桥梁。

## 3、操作系统概述

操作系统（Operating System，简称OS）是管理和控制计算机硬件与软件资源的计算机程序，是直接运行在“裸机”上的最基本的系统软件，任何其他软件都必须在操作系统的支持下才能运行。

![image-20210906105455952](../images/Linux/media/image-20210906105455952.png)

## 4、操作系统分类

![image-20210906105816844](../images/Linux/media/image-20210906105816844.png)

由于Linux是开源免费的，而且相比Windows/Mac更加安全、稳定。所以大数据组件都是基于Linux系统安装的，所以，Linux操作系统是我们大数据学习的必备技能。

## 5、小结

知道计算机的组成部分

知道为什么要学习Linux操作系统

# 二、Linux操作系统概述

## 1、Linux起源

Linux创始人——林纳斯 · 托瓦兹

Linux 诞生于1991年，作者上大学期间实现的

Linux的特点：开源、免费、拥有最为庞大的源码贡献者

Linux的吉祥物是企鹅（因为林纳斯小时候被企鹅咬过，印象深刻）

![image-20210906110452331](../images/Linux/media/image-20210906110452331.png)

## 2、Linux 的含义

狭义：由Linus 编写的一段内核代码。

广义：广义上的Linux 是指由Linux内核衍生的各种Linux发行版本。

## 3、Linux发行版

![image-20210906110721658](../images/Linux/media/image-20210906110721658.png)

## 4、小结

了解Linux的含义

知道Linux主流的发行版

# 三、虚拟机与Linux系统安装

## 1、系统的安装方式

Linux操作系统也有两种安装方式：① 真机安装 ② 虚拟机安装

## 2、虚拟机概念

什么是虚拟机？

虚拟机，有些时候想模拟出一个真实的电脑环境，碍于使用真机安装代价太大，因此而诞生的一款可以模拟操作系统运行的==软件==。

虚拟机目前有2 个比较有名的产品：==vmware 出品的vmware workstation==、oracle 出品的virtual Box。

![image-20210906111159808](../images/Linux/media/image-20210906111159808.png)

## 3、虚拟机的安装

> 强调：安装后尽量不要卸载，否则后果自负！！！

![image-20210906111353762](../images/Linux/media/image-20210906111353762.png)

软件没有什么过多的注意事项，直接双击软件包进行安装即可。但是需要特别注意：当VMware软件安装完毕后，在计算机的网络中会出现两张虚拟网卡（VMnet1和VMnet8）

![image-20200630174304731](../images/Linux/media/image-20200630174304731.png)

## 4、Linux系统安装

第一步：解压人工智能虚拟机

![image-20220216184746871](../images/Linux/media/image-20220216184746871.png)

第二步：找到解压目录中的node1.vmx

![image-20220216184813243](../images/Linux/media/image-20220216184813243.png)

鼠标右键，使用VMware Workstation打开

第三步：启动操作系统

![image-20210906112747987](../images/Linux/media/image-20210906112747987.png)

选择我已移动该虚拟机

![image-20210906112916587](../images/Linux/media/image-20210906112916587.png)

默认管理员账号：root

输入默认密码：123456

![image-20210906113257484](../images/Linux/media/image-20210906113257484.png)

单击登陆，进入CentOS7操作系统，如下图所示：

![image-20210906113337265](../images/Linux/media/image-20210906113337265.png)

如果想从Linux系统切换回Windows系统，则可以使用快捷键Ctrl + Alt。

# 四、Linux连接工具使用

## 1、为什么要使用远程连接工具？

答：因为一般的AI人工智能的服务器都是放在机房的，我们不可能每天都跑到机房里去操作这些机器。所以，我们需要使用远程工具，通过网络连接到机房里的机器。

![image-20210906113433486](../images/Linux/media/image-20210906113433486.png)

## 2、虚拟机网络配置

我们需要远程连接虚拟机，如果使用随机IP我们再重启或更改网络环境后，IP会随机变化，需要频繁修改网络连接配置，为方便学习，我们将其修改为固定IP。

![image-20210906113936884](../images/Linux/media/image-20210906113936884.png)

选择NAT模式，修改子网IP为指定网段，此处IP设置为192.168.88.0，点击应用。

![image-20210906113956841](../images/Linux/media/image-20210906113956841.png)

选择DHCP设置，将起始IP设置为192.168.88.1，终止IP设置为192.168.88.254，点击确定。

![image-20210906114011791](../images/Linux/media/image-20210906114011791.png)

点击NAT设置，将网关IP设置为192.168.88.1，点击确定后，返回上一级点击确定，设置生效。

![image-20210906114033167](../images/Linux/media/image-20210906114033167.png)

## 3、获取Linux操作系统IP地址

① 打开终端

② 在终端中，输入一个命令：`ip 空格 a`命令

> ip命令，a是一个参数，代表all，显示所有网卡的IP信息

③ 查看一个叫做ens33网卡的IP地址，这个地址叫做物理IP或者简单理解就是你插网线那个网卡的IP地址

![image-20211211150117256](../images/Linux/media/image-20211211150117256.png)

④ 在Windows操作系统中，远程测试一下这个IP地址是否可以连接（ping命令）

Windows电脑：Windows键 + R，输入cmd就可以打开DOS窗口了

![image-20211211150455954](../images/Linux/media/image-20211211150455954.png)

![image-20211211150647440](media/image-20211211150647440.png)

## 4、聊一聊Linux系统账号

问题：是不是有了IP地址，我们可以连接Linux操作系统了

答：IP只能保障两台计算机互相通信，如果想进行连接，除了有Linux的IP地址以外，还需要一个Linux的账号与密码。

账号一般分为两大类：① 普通账号（如itcast账号） ② 超级管理员（如root账号）

① 普通账号作用：一般可以用于登录操作系统，可以对自己的家目录（文件夹）进行管理

② 超级管理员作用：包括系统管理、所有用户的管理、软件的安装卸载、包括网络的配置等等，都可以通过root超级管理员进行实现。

咱们已经安装好的系统，可以通过itcast或者root账号进行管理。

默认情况下，Linux系统中的两个账号（itcast与root）密码都是`123456`

> 在学习阶段，推荐使用root账号进行远程管理。但是操作时一定要特别小心。

问题：如何使用命令从itcast普通账号切换到root管理员账号

答：可以使用`su`命令

```powershell
[itcast@node1 ~]$ su - root
密码：输入123456即可（但是输入的字符你看不见）

说明：以上命令的主要功能是从itcast普通账号切换到root超级管理员，要输入密码。
-横岗说明：-横岗在Linux操作系统中代表，切换用户的同时，把当前位置也切换到root管理员的家目录

[itcast@node1 ~] : 波浪线代表itcast的家
[root@node1 ~] : 波浪线代表root的家
```

## 5、安装finalshell远程连接软件

finalshell是一款强大的远程终端连接工具。可以用于远程连接Linux系统，通过远程方式执行命令完成任务。

![image-20211211155117052](../images/Linux/media/image-20211211155117052.png)

> 详细安装请参考安装视频

## 6、建立连接

第一步：启动finalshell软件

![image-20211211155300717](../images/Linux/media/image-20211211155300717.png)

第二步：单击文件夹图标，如下图所示：

![image-20211211155419765](../images/Linux/media/image-20211211155419765.png)

第三步：填入Linux服务器的IP地址、管理员用户名root以及管理员root密码（默认123456）

![image-20211211155937153](../images/Linux/media/image-20211211155937153.png)

单击确认，配置完成

第四步：双击CentOS7连接，开始连接Linux服务器

![image-20211211160033592](../images/Linux/media/image-20211211160033592.png)

连接成功后，接收并保存秘钥（下次再次发起连接就不需要重复输入账号和密码了）

![image-20211211160152738](../images/Linux/media/image-20211211160152738.png)

最终结果：

![image-20211211160213817](../images/Linux/media/image-20211211160213817.png)

## 7、软件界面与使用说明

![image-20211211161414397](../images/Linux/media/image-20211211161414397.png)

## 8、小结

为什么需要finalshell？

掌握虚拟机网络配置

# 五、Linux的目录结构

## 1、Linux目录与Windows目录区别

Linux的目录结构是一个树型结构
Windows 系统 可以拥有多个盘符, 如 C盘、D盘、E盘
Linux 没有盘符 这个概念, 只有一个根目录 /, 所有文件都在它下面

![image-20210906114619712](../images/Linux/media/image-20210906114619712.png)

## 2、常见目录介绍（记住重点）

| 目录  | 作用                                                         |
| ----- | ------------------------------------------------------------ |
| /bin  | 二进制命令所在的目录(普通命令 => 普通用户itcast和超级管理员root) |
| /boot | 系统引导程序所需要的文件目录，相当于Windows中的C盘           |
| /dev  | 设备软件目录，磁盘，光驱 => /dev/sr0                         |
| /etc  | 系统配置，启动程序                                           |
| /home | 普通用户的家，目录默认数据存放目录                           |
| /lib  | 共享库文件和内核模块存放目录，软件安装、运行依赖库文件.a、.so文件 |
| /mnt  | 临时挂载储存设备的挂载点，插入u盘、移动硬盘 => 先挂载 => /mnt中访问 |
| /opt  | 额外的应用软件包， 安装qq、游戏、wps办公软件                 |
| /proc | 操作系统运行时，进程信息和内核信息存放在这里                 |
| /root | Linux超级权限用户root的家目录                                |
| /sbin | 和管理系统相关的命令，【超级管理员用】，s = super超级        |
| /tmp  | 临时文件目录，这个目录被当作回收站使用                       |
| /usr  | 用户或系统软件应用程序目录，类似Windows中的Program files     |

① 普及概念：`用户的家目录`

普通用户：itcast，普通用户的家 => /home，如itcast家目录 => /home/itcast文件夹

超级管理员：root，超级管理员的家 => /root

② 普及概念：`系统配置文件目录`

/etc ：与操作系统相关，系统软件相关，比如网卡配置 => 88.100 ~ 88.200

③ 普及概念：`/tmp目录`

临时文件目录，类似Windows中的垃圾回收站。

④ 普及概念：`/usr目录`

Linux系统中的程序目录，安装软件、程序默认都会自动安装到此目录，类似Windows中的Program files文件夹

## 3、小结

了解Linux系统的目录结构

能够说出几个重点目录的作用

# 六、Linux常见命令

## 1、命令结构

```powershell
command [-options] [parameter]

说明:
command : 命令名, 相应功能的英文单词或单词的缩写
[-options] : 选项, 可用来对命令进行控制, 也可以省略
parameter : 传给命令的参数, 可以是 零个、一个 或者 多个
```

命令有三种情况：

① 只有命令，没有选项也没有参数

② 除了命令以外，还有选项，但是没有参数

③ 除了命令以外，还要有选项和参数

## 2、ls命令

 作用 ：ls 是英文单词list的简写, 其功能为列出目录的内容，是用户最常用的命令之一

格式

```powershell
ls [选项] [路径]
```

ls常用选项

| **选项** | **含义**                                              |
| -------- | ----------------------------------------------------- |
| **-a**   | all所有, 显示指定目录下所有子目录与文件, 包含隐藏文件 |
| **-l**   | 以列表方式显示文件的详细信息                          |
| **-h**   | 配合 -l 以人性化的方式显示文件大小（文件大小 + 单位） |

案例演示：

```powershell
ls           #查看当前目录内容 (缺点: 隐藏文件看不到,以 .开头的文件) ！
ls -a        #查看当前目录内容 ,包括隐藏文件 
ls –al       #查看目录内容的详细信息(查看文件类型、权限、大小等) 
ls -lh       #查看目录内容的详细信息,以K,M,G方式显示文件大小 
ls /root     #查看/root目录下内容

快捷键 ll 相当 ls
ll           #等价于ls -l
```

## 3、cd命令

作用：cd 是英文单词 change directory 的缩写, 其功能为 更改当前的工作目录, 也是用户最常用的命令之一。

| 命令    | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| cd      | 切换到用户主目录（root用户主目录是/root,其他用户是/home/用户名） |
| cd 目录 | 切换到指定目录下                                             |
| cd ..   | 切换到上级目录                                               |

>  提示：执行 pwd 指令可立刻得知您目前所在的工作目录的绝对路径名称。

案例演示：

```powershell
cd            #回到用户主目录
cd test       #切换到当前目录下的test目录（相对路径） 
cd /root/test #切换到指定目录（绝对路径）
cd ..         #回到上一级目录 
cd ../..      #回到上上一级目录
cd ../dir     #回到上一级的dir目录 
```

扩展：路径概念

① 绝对路径

代表从==/根目录==开始一级一级向下查找，直到找到我们想要访问的目录位置。

![image-20220217111717873](../images/Linux/media/image-20220217111717873.png)

绝对路径 => /usr/local

绝对路径 => /home/bob



② 相对路径(必须要有一个参考点，一般为用户当前所在路径)

同级关系：只需要通过./或者直接输入文件或文件夹名称即可

上级关系：在Linux系统中，我们可以通过..来访问当前路径的上一级

当前位置：/usr目录下面，切换到/根目录的下方，可以使用..来实现

下级关系：可以使用文件夹名称/

## 4、mkdir命令

作用：mkdir命令用于创建目录

```powershell
mkdir [-p] dirName

参数：
-p：一次创建多级目录
```

案例演示：

```powershell
mkdir ai 		  #创建单级目录 
mkdir -p aaa/bbb/ccc  #创建多级目录 
```

## 5、touch命令

作用：touch命令创建文件

格式：

```powershell
touch 文件名
```

案例演示：

```powershell
touch a.txt  	   #在当前目录创建a.txt文件 
touch /root/a.txt  #在/root目录创建a.txt文件
```

## 6、rm命令

作用：rm命令用于删除文件或者目录

格式：

```powershell
rm [参数] 文件或者目录名
```

| 参数 | 英文             | 含义                                           |
| ---- | ---------------- | ---------------------------------------------- |
| -f   | force (强制)     | 强制删除,忽略不存在的文件或目录, 无需提示      |
| -r   | recursive (递归) | 递归地删除目录下的内容, 删除目录时必须加此参数 |

案例演示：

![image-20210906115918008](../images/Linux/media/image-20210906115918008.png)

扩展：一个非常非常危险的命令

```powershell
# rm -rf /*
rm代表删除
-rf代表强制删除不提示
/代表根目录
*代表通配符，匹配所有文件

最终以上命令就代表删除根目录下的所有文件
```

## 7、cp命令

作用：cp命令用来实现文件或者目录的复制

格式：

```powershell
cp 源路径 目标路径
```

案例演示：

```python
cp a.txt dir1    #将a.txt复制到dir1目录
cp a.txt b.txt   #将a.txt复制为b.txt
cp –r dir dirx   #复制目录
```

## 8、mv命令

作用：mv命令用于文件、目录的移动和重命名

格式：

```powershell
mv 原路径 目标路径
```

移动案例演示：

```powershell
mv a.txt dir  #将a.txt移动到dir目录
mv dir2 dir   #将dir2目录移动到dir目录
```

重命名案例演示：

```powershell
mv a.txt b.txt  #将a.txt重命名为b.txt
mv dir2 dir22   #将dir2目录重命名为dir22
```

## 9、cat命令

作用：用于显示文件内容

格式：

```powershell
cat 文件名称
```

案例演示：

```powershell
cat /root/initial-setup-ks.cfg
```

## 10、more命令

作用： 用于显示文件内容，可以按页或者按行显示文件内容

格式：

```powershell
more 文件名称

快捷键
Enter: 向下n行, 需要定义, 默认为1行
空格键: 向下滚动一屏 或 Ctrl + F
B键: 返回上一屏 或 Ctrl+B 
q: 退出more
```

案例演示：

```powershell
more /root/initial-setup-ks.cfg
```

## 11、ps命令

作用：ps命令用来列出系统中当前运行的进程

格式

```powershell
ps [options]
```

案例演示：

```powershell
ps -ef #查看正在运行的所有进程
```

## 12、kill命令

作用：kill命令用于终止执行中的程序

格式：

```powershell
kill [参数] [进程号]
```

案例：

```powershell
kill -9 12345 #杀死pid为12345的进程
```

## 13、ifconfig命令

作用：ifconfig命令用来查看ip地址

格式：

```powershell
ifconfig
```

案例演示：

```powershell
[root@node1 ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.88.161  netmask 255.255.255.0  broadcast 192.168.88.255
        inet6 fe80::20c:29ff:fe49:b3ec  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:49:b3:ec  txqueuelen 1000  (Ethernet)
        ...
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 90  bytes 17886 (17.4 KiB)
     	...
```

## 14、clear命令

作用：clear命令用来清屏，可以使用Ctrl + L 来替换

格式：

```powershell
clear
```

案例演示：

```powershell
[root@node1 ~]# clear
```

## 15、重启与关机命令

重启：

```powershell
reboot
```

关机：

```powershell
shutdown -h now : 立刻关机(断电关机)
halt : 立刻关机 (不断电关机)
```

## 16、which命令

作用：which显示执行命令的绝对位置

## 17、hostname命令

作用：查看主机名称

```powershell
hostname
```

## 18、grep命令

作用：对文件内容进行检索

案例演示：

```powershell
grep lang anaconda-ks.cfg 		#在文件中查找lang
grep a anaconda-ks.cfg --color  #在文件中查找a,高亮显示
```

选项：

-n ：number缩写，代表显示信息时，显示行号

## 19、|管道

作用：管道命令主要功能就是将管道前面的命令的执行结果传递给管道后面的命令作为参数

案例演示：

```powershell
ps  -ef| grep mysql : 在所有进程中快速找到包含mysql内容的进程
```

## 20、useradd命令

作用：创建账号

案例演示：

```powershell
useradd itheima  # 创建账号
passwd  itheima  # 设置密码
```

注：在Linux操作系统中，虽然我们通过useradd命令可以快速创建一个账号，但是这个账号默认没有密码，所以不能进行登录操作。如果想进行登录，必须为这个账号添加一个密码！

## 21、userdel命令

作用：删除账号

案例演示：

```powershell
userdel -r itheima
```

## 22、tar命令

作用：压缩文件与解压缩文件

```powershell
tar [选项]
```

选项说明：

| 选项 | 解释                         |
| ---- | ---------------------------- |
| -c   | 创建一个新tar文件            |
| -v   | 显示运行过程的信息           |
| -f   | 指定文件名                   |
| -z   | 调用gzip压缩命令进行解、压缩 |
| -x   | 解包                         |

解压：

```powershell
tar -zxvf redis-3.2.8.tar.gz  			  #将文件解压到当前目录
tar -zxvf redis-3.2.8.tar.gz -C /root/dir #将文件解压到指定目录
```

压缩：

```powershell
tar -cvf  test.tar /root/test     #打包tar -xf test.tar  # 解tar包
tar -xf test.tar -C   /export  	  #解压到指定目录
tar -czvf test.tar.gz /root/test  #打包并压缩
```

## 23、su命令

作用：切换（用户）账号

```powershell
# su - itheima
```

-横岗：代表切换用户的同时，把当前的目录切换到用户的家目录

# 七、Linux的vi/vim编辑器

## 1、vi/vim编辑器介绍

vi是visual interface的简称, 是Linux中最经典的文本编辑器

vi的核心设计思想：让程序员的手指始终保持在键盘的核心区域, 就能完成所有编辑操作

vi的特点：

* 只能是编辑文本内容, 不能对字体段落进行排版
* 不支持鼠标操作
* 没有菜单
* 只有命令

vim 是从vi发展出来的文本编辑器, 支持代码补全、编译及显示效果等方面编程的功能提别丰富, 在程序员中被广泛使用, 被称为编辑器之神。

![image-20210906150233075](../images/Linux/media/image-20210906150233075.png)

## 2、打开文件

```powershell
vi a.txt          #直接打开文件
vim a.txt         #vim是vi的增强版
vim +10 a.txt  	  #直接打开文件，并定位到第10行
```

## 3、VIM编辑器的三种模式(重点)

![image-20210906150356115](../images/Linux/media/image-20210906150356115.png)

## 4、命令模式相关命令

| **命令** | **功能**                    |
| -------- | --------------------------- |
| o        | 在当前行后面插入一空行      |
| O        | 在当前行前面插入一空行      |
| dd       | 删除光标所在行              |
| ndd      | 从光标位置向下连续删除 n 行 |
| yy       | 复制光标所在行              |
| nyy      | 从光标位置向下连续复制n行   |
| p        | 粘贴                        |
| u        | 撤销上一次命令              |
| gg       | 回到文件顶部                |
| G        | 回到文件末尾                |
| /str     | 查找str                     |

## 5、底行模式相关命令

| **命令**          | **功能**                      |
| ----------------- | ----------------------------- |
| :w  文件          | 另存为                        |
| :w                | 保存(ctrl + s)                |
| :q                | 退出, 如果没有保存,不允许退出 |
| :q!               | 强行退出, 不保存退出          |
| :wq               | 保存并退出                    |
| :x                | 保存并退出                    |
| Shift  + z + z    | 保存退出                      |
| :set  nu          | 设置行号                      |
| :%s/旧文本/新文本 | 文本替换                      |
| :noh              | 取消高亮                      |

## 6、小结

了解VIM编辑器的三种模式