---
title: ubuntu常用命令
date: 2023/7/8 19:46:25
comments: true
tags: 
- ubuntu
categories: 
- 计算机
- ubuntu
---

# ubuntu常用命令

## 文件带锁

```
# 单个文件被锁的情况
sudo chmod 777 filename
# 文件夹里面的内容会全部带锁的情况
sudo chown -R username filename
```

# 创建文件

在Ubuntu中可以使用命令行或图形界面的方式来创建文件。

1. 使用命令行方式：
   - 打开终端。
   - 使用`touch`命令创建一个空文件，如：
     ```
     touch filename.txt
     ```
     这将创建一个名为filename.txt的空文件。
   - 使用文本编辑器（如`nano`或`vi`）创建并编辑文件，如：
     ```
     nano filename.txt
     ```
     这将在nano编辑器中打开一个名为filename.txt的文件，你可以输入并保存内容。

2. 使用图形界面方式：
   - 打开文件管理器（如Nautilus）。
   - 导航到你想要创建文件的目录。
   - 在目录中右键单击，选择"创建文档"或"新建文件"选项。
   - 输入文件名（包括文件扩展名）并按Enter键。
   - 双击新创建的文件，在默认文本编辑器中打开，并输入内容。

这些方法都可以在Ubuntu中创建文件。你可以根据你的喜好和需求选择使用命令行或图形界面方式。