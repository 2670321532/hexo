---
title: github远程仓库保存hexo
date: 2024/5/31 20:46:25
comments: true
tags: 
- blog
- hexo
categories: 
- 计算机
- hexo
- GitHub远程保存Hexo
---

### 一、在 GitHub 上创建新项目仓库

1. **登录 GitHub**：访问 [GitHub](https://github.com/) 并登录你的账户。
2. 创建新仓库：
   - 点击右上角的加号（`+`）图标，选择“New repository”。
   - 填写仓库名称（例如 `my-hexo-blog`）。
   - 添加描述（可选）。
   - 选择“Public”或“Private”作为仓库的可见性。
   - 确保不勾选“Initialize this repository with a README”选项。
   - 点击“Create repository”按钮。

### 二、在本地配置 Hexo 项目并推送到 GitHub

1. **打开终端**：打开你的终端或命令行工具。

2. 进入 Hexo 项目目录：

   使用 

   ```
   cd
   ```

    命令进入你的 Hexo 项目文件夹。例如：

   ```
   cd path/to/your/hexo-project
   ```

3. 初始化 Git 仓库

   ：如果你的 Hexo 项目目录还没有初始化 Git 仓库，可以执行以下命令：

   ```
   git init
   ```

4. 添加远程仓库

   ：将你在 GitHub 上创建的新仓库添加为远程仓库。替换 <username>和 <repository>为你的 GitHub 用户名和仓库名称：

   ```
   git remote add origin https://github.com/<username>/<repository>.git
   ```

5. 生成静态文件：在 Hexo 项目目录中运行以下命令生成静态文件：

   ```
   hexo generate
   ```

6. 将生成的文件复制到新的发布目录：将生成的静态文件（通常在 public/目录）复制到一个新的发布目录（例如 deploy/

   ```
   cp -r public/* ../deploy/
   ```

7. 进入发布目录：切换到发布目录：

   ```
   cd ../deploy
   ```

8. 添加文件到 Git

   ```
   git add .
   ```

9. 提交更改

   ```
   git commit -m "Initial commit of Hexo blog"
   ```

10. 推送到 GitHub

    ```
    git push -u origin master
    ```

### 三、配置 GitHub Pages（如果需要）

1. 在 GitHub 仓库中启用 GitHub Pages

   ：

   - 进入你在 GitHub 上创建的仓库页面。
   - 点击“Settings”。
   - 在左侧菜单中选择“Pages”。
   - 在“Source”部分，选择 `main` 分支（或 `master`，取决于你的默认分支名称），并点击“Save”。
   - GitHub 将为你生成一个 GitHub Pages URL，你可以在几分钟后访问你的 Hexo 博客。



### 四.要配置 Git 的用户名和电子邮件地址

这是因为 Git 需要知道你的身份信息才能进行提交。你需要配置 Git 的用户名和电子邮件地址。这些信息将包含在每个提交中，以标识提交的作者。

你可以按照提示配置全局用户名和电子邮件地址，或者仅为当前仓库配置。以下是两种配置方法：

#### 全局配置（适用于所有仓库）

全局配置将应用于你计算机上的所有 Git 仓库。

1. 设置全局电子邮件地址：

   ```
   git config --global user.email "you@example.com"
   ```

2. 设置全局用户名：

   ```
   git config --global user.name "Your Name"
   ```

将 `"you@example.com"` 替换为你的电子邮件地址，将 `"Your Name"` 替换为你的名字或 GitHub 用户名。

#### 局部配置（仅适用于当前仓库）

如果你只想为当前仓库配置用户名和电子邮件地址，可以使用以下命令：

1. 进入你的 Hexo 项目目录：

   ```
   cd path/to/your/hexo-project
   ```

2. 设置当前仓库的电子邮件地址：

   ```
   git config user.email "you@example.com"
   ```

3. 设置当前仓库的用户名：

   ```
   git config user.name "Your Name"
   ```

#### 配置完成后重新提交

配置完成后，你可以再次尝试提交：

1. 添加文件到暂存区（如果还没有）：

   ```
   git add .
   ```

2. 提交更改：

   ```
   git commit -m "Initial commit of Hexo blog"
   ```

### 完成！

现在，你的 Hexo 博客应该已经成功上传到 GitHub 并可以通过 GitHub Pages 访问。如果有任何问题，检查终端输出的错误信息，或参考 GitHub 和 Hexo 的文档进行排查。

