---
title: git使用
date: 2023/7/8 19:46:25
comments: true
tags: 
- git
- 版本管理工具
categories: 
- 计算机
- git
---

# GIT版本管理工具教程

## Git初始化

下载安装， 下载地址： https://git-scm.com/downloads 每个系统的都有（linux、mac、windows等），看官网的安装教程，很详细，

此处我以windows来练习

首先创建一个文件夹，这个文件夹就是我们将来通过git来管理的所有文件的存放地点 。

在文件夹中右键 使用Git Bash

在弹出的窗口中执行初始化指令，让git帮我们对这个文件夹中的所有文件以及文件夹进行管理

```
git init  
# 创建git版本管理的本地仓库
```

产生的.git文件夹用来存放你管理的文件的所有版本以及git配置相关的内容，不要轻易动它

## 简单指令使用

### 基本操作

```
git status 查看仓库中所有的文件夹和文件的状态
git add supercrm  让git管理单独的文件夹或者文件
git add .  管理所有文件和文件夹

配置用户名和邮箱
$ git config --global user.name <用户名>
$ git config --global user.email <邮箱地址>
例如：
$ git config --global user.name "吴超"
$ git config --global user.email "1069696250@qq.com"


然后就可以提交版本了，看指令
git commit -m '描述信息'
例如： git commit -m 'v1版本'
```

管理之后进行二次开发，修改一些文件之后：

```
git add supercrm
git commit -m 'v2版本'
```

查看日志

```
git log
```

### 简单总结

```
 进入要管理的目录
 git init初始化  即：让git管理我们当前的文件夹
 git status 检测当前文件夹中的文件状态
 三种颜色的变化
    a 红色：新增文件或者修改的老文件 --> 执行git add .（或者单个文件或文件夹的名称）  
    b 绿色：git已经管理起来了 --> 执行git commit -m '描述信息'
    c 白色：生成版本了

好，之后我们会细说这几个颜色到底还有什么意义
 git log 查看版本记录
```

## Git进阶

### Git三大区域

介绍： 作区(写代码的地方)—git add暂存区(临时存储)—git commit本地库(历史版本)

```
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向
master的一个指针叫HEAD。

文件往Git版本库里添加的时候，是分两步执行的：
第一步用git add把文件添加进去，实际上就是把文件修改添加到暂存区。
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

创建Git版本库时，Git自动为我们创建了唯一一个master分支，git commit就是往master分支上提交更改。add需要提交的文件修改通通放到
暂存区，然后commit可一次性提交暂存区的所有修改
```

![](../images/git/GIT three areas.png)

### Git回滚

 假如我们现在写了三个版本了，但是你发现第三个版本有问题，或者说被迫的下线第三个版本添加的新功能，那么我们是不是要将代码回到第二个版本的状态啊，如果我们自己手动去修改是不是就非常的麻烦了，所以此时就用到我们git回滚功能了 。

![数据迁移](../images/git/版本号.png)



Git回滚操作指令

```
git reset --hard 版本号
例如：git reset --hard a3c69761b4ecd8b23c392315cd245f2939024882 (第二个版本的版本号)
```

 不过，后来你又觉得第三个版本的功能还是挺好的，接着拿回来用吧，但是你已经回滚到第二个版本了啊，这可怎么办？看操作：

 先执行git log，你发现，git log里面没有显示我们原来的第三个版本，对不对，此时我们不能用这个指令来查看了，需要下面这个指令：

```
git reflog  #也是查看日志，但是包括回滚操作的版本
再通过git reset --hard 版本号来回滚
```

 Git的强大之处，能够让我们在任意版本之间来回切换。

 我们接着学两个指令

```
git checkout -- 文件名  #将文件从以修改的工作区回滚到未修改的状态
```

 如果我们将修改i的文件已经添加到了暂存区了，又怎么回滚呢？看指令

```
git reset HEAD 文件名
```

 如果想让他再回到未修改时的状态，那么就又用到了我们那个git checkout – 文件名，那个指令了 。

 关于回滚，git里面还有几个其他的指令，就不一个一个的演示了，大家看图就明白了：

![数据迁移](../images/git/%E5%9B%9E%E6%BB%9A%E6%BC%94%E7%A4%BA.png)

#### 指令总结

```
git init
git add
git commit
git log   不包含回退记录的
git reflog  包含回退记录的
git reset --hard 版本号
```

### Git分支

 分支可以给使用者提供多个开发环境，也就是说可以将你的工作从主线中分离出来，以免影响开发主线，等分支的代码开发完之后，再合并到主线代码上，就比如说，我们写了一个毕业论文，大致的流程写完了，但是我们可能觉得某些地方写的太少了（添加新功能），需要丰富一下，或者有些地方可能写的有问题需要调整一下（之前的代码有bug，需要修改），那么我们怎么做呢，是不是会复制一份这个论文，然后再修改，改完之后如果没有什么问题，就将改完之后的作为了最新的版本（分支上添加了新功能或者修复了bug，然后进行分支合并）。

 大家在这里先不用去考虑公司里面到底是怎么使用git来进行工作的，我们首先先来看看，如果你在自己的电脑上开发程序，用git是怎么个流程，怎样开分支，分支是个什么样子？

 比如，我们现在的代码开发到了第三个版本，之前我们没有说什么分支的概念，其实我们开发代码的时候，默认的分支叫做主分支(master分支)，只是我们一直还不知道。

![数据迁移](../images/git/Git%E5%88%86%E6%94%AF.png)

#### 指令总结

```
git branch    查看当前分支
git branch dev  创建一个名为dev的分支
git checkout dev  将工作切换到dev分支上

git checkout -b dev #创建并切换到dev分支上，和上面两个指令的效果一样

git checkout master
git merge dev    #分支合并---首先切换到master分支，然后在master分支上执行merge指令来合并dev分支的代码

git branch -d bug  删除bug分支
```

 比如我当前的代码只到了test3，3版本，我想添加一个新的功能test4，那么我就创建了一个dev分支，并在dev分支上添加了test4，比如说test4需要打印两行内容，但是我现在写了一行内容的时候，发现之前线上使用的代码(线上使用的代码一般是master分支上的)，出现了bug，那么我们需要切换到master分支，并且在master分支上再创建一个bug分支，在bug分支上修复bug，修复完成之后，需要合并到master分支上，合并之后的版本我们暂且称为5版本，记着，5版本的代码和dev开分支时的3版本代码是有些变动的，因为修复了bug，但是dev分支上还是使用的master分支上的v3版本进行的新功能的开发，那么bug修复完之后，我们现在又要回到dev分支上继续新功能的开发，开发完成之后，需要合并到master分支上，合并的时候，你会发现报了一个错误，其实也不是错误，就是提示你，代码有冲突，这冲突怎么来的，你想，master分支已经到了c5版本，但是dev分支上的代码还是从master分支的c3版本的基础进行添加新功能的，所以合并的时候c3版本的其他代码和它c5版本的代码本身就有一些不一样的地方（就是那个bug修复的地方），所以出现了冲突的问题，那么怎么办呢，没办法，我们只能手动来修复冲突，那么怎么修复呢，git会将所有的冲突全部标记在你的代码文件中，有冲突的方法，找到它手动修改一下就可以了。其实，只要我们两个分支中的相同的文件的同一行出现了不同的内容，合并时就会出现冲突。看一下冲突的报错是什么样子的：

![数据迁移](../images/git/Git%E5%88%86%E6%94%AF1.png)

出现了bug，我们看看bug在哪里，其实git会将冲突在你的代码文件中标识出来，看图：

![数据迁移](../images/git/Git%E5%88%86%E6%94%AF2.png)

这里提示你了，dev分支上是哪些内容，master分支上是哪些内容，我们把没用的删除就可以了，然后提交一个新的版本，这样就完成了分支代码合并

### Git工作流

![数据迁移](../images/git/git%E5%B7%A5%E4%BD%9C%E6%B5%81.png)

 公司一般最少有两个分支，master只保留正式(线上)版本，dev(develop)分支，开发都在dev分支上搞。

## Github代码管理仓库

 我们做开发的时候，写程序，可能会有多个人一起开发，或者你自己有多个电脑，家里一个电脑，办公室一个电脑，但是你如果刚开始的代码都是在家里的电脑写的，然后你到了公司，你想继续开发你的程序，那么就需要你自己来回的拷贝自己的代码，并随身携带，非常麻烦，你说对不对，所以现在就出现了代码网络托管站(就类似于行李托管站一样)，可以帮你保存你的代码，以及各个版本的代码和所有分支，这样的话，你在家里开发完了之后，把代码放到托管站，就不用自己随身携带了，等你到了公司，使用公司的电脑开发的时候，就可以直接通过网络托管站把自己已经开发好的代码拉下来到自己的本机，然后继续开发，开发完了之后，在交给托管站托管，这样就方便多了，有很多这样的托管站，比如今天我们要说的GIthub，还有GitLab、码云、开源中国、CSDN等都在做代码托管平台。

![数据迁移](../images/git/%E6%89%98%E7%AE%A1%E7%AB%99.png)

使用Github有这么几步：

- 注册Github账号

- 创建仓库

- 本地代码推送到仓库

  好，那么我就来看看一看具体怎么玩：

### 第一步：注册Github账号

这个就不带着大家注册了，看图，网址： https://github.com/

![数据迁移](../images/git/%E6%B3%A8%E5%86%8Cgithub.png)

### 第二步：创建仓库

 也就是我们说的托管站里面开辟一个自己的代码托管的空间，看操作

![数据迁移](../images/git/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)

 然后会看到下面的页面，看介绍

![](../images/git/创建仓库介绍.png)

创建好之后我们会看到下面这个页面，其实在GitHub这个托管站上，就相当于我们创建了一个叫做dbhot(就上面我起的那个仓库名称)的文件夹，用来管理我们的项目。

![数据迁移](../images/git/%E4%BB%93%E5%BA%93%E5%9C%B0%E5%9D%80.png)

### 第三步：Github保存代码

 将我们的代码和分支推送到GitHub上保存

 打开我们的终端，也就是我们那个git bash，查看一下状态。

![数据迁移](../images/git/Github%E4%BF%9D%E5%AD%98%E4%BB%A3%E7%A0%81.png)

然后看一下推送代码的指令：

![数据迁移](../images/git/%E6%8E%A8%E9%80%81%E4%BB%A3%E7%A0%81%E7%9A%84%E6%8C%87%E4%BB%A4.png)

 查看一下当前的分支

![数据迁移](../images/git/%E6%9F%A5%E7%9C%8B%E5%BD%93%E5%89%8D%E5%88%86%E6%94%AF.png)

 然后执行指令：

![数据迁移](../images/git/%E8%BF%9E%E6%8E%A5%E8%BF%9C%E7%A8%8B%E5%9C%B0%E5%9D%80.png)

 然后执行它：

![数据迁移](../images/git/%E6%8E%A8%E9%80%81%E6%8C%87%E4%BB%A4.png)

会给你弹出一个窗口，让你输入GitHub的账号和密码，这里我的截图失败了，导致大家在这里看不到效果了，但是没关系，你应该可以搞定的。然后接着看：

![数据迁移](../images/git/%E6%8E%A8%E9%80%81%E5%87%BA%E7%8E%B0%E9%94%99%E8%AF%AF.png)

 等代码都推送成功了之后，我们来GitHub上刷新一下我们的仓库页面，就会看到变化了：

![数据迁移](../images/git/%E5%88%B7%E6%96%B0%E4%BB%93%E5%BA%93.png)

推送一下dev分支到GitHub上：

![数据迁移](../images/git/dev%E5%88%86%E6%94%AF%E5%88%B0GitHub.png)

刷新我们的GitHub页面，就看到有两个分支了

![数据迁移](../images/git/git%E4%B8%8A%E6%9C%89%E4%B8%A4%E4%B8%AA%E5%88%86%E6%94%AF.png)

### 第四步： 拉取GitHub上的代码继续开发

 换一个电脑，然后拉取GitHub上的代码继续开发，我这里简单模拟一下，就在本地重新创建一个文件夹了，然后在这个文件夹里面来搞我们的代码

 比如：我又创建了一个gitpro2文件夹，然后在这个目录里面使用我们的git bash：

![数据迁移](../images/git/%E5%88%9B%E5%BB%BAgitpro2%E6%96%87%E4%BB%B6%E5%A4%B9.png)

 在这里面克隆一下远程GitHub上的代码到我们的这个文件夹里面：

 先看一下我们远程仓库的地址是什么，看GitHub：

![数据迁移](../images/git/%E6%9F%A5%E7%9C%8B%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E5%9C%B0%E5%9D%80.png)

然后执行下面的指令：

```
git clone 地址
例如： git clone https://github.com/clschao/dbhot.git
```

 然后整个仓库就被我们克隆下来了（包括所有分支，所有代码，所有版本），在我们的gitpro2文件夹里面了，然后我们需要进入到仓库里面才能看到我们的代码：

![数据迁移](../images/git/%E6%9F%A5%E7%9C%8B%E6%96%87%E4%BB%B6%E5%A4%B9%E4%BB%A3%E7%A0%81.png)

### 第五步：换一个电脑继续开发

#### 指令总结

```
上传代码
1. 给远程仓库起名
    git remote add origin 远程仓库地址
2. 向远程推送代码
    git push -u origin 分支

在新电脑上第一次获取代码
1. 克隆远程仓库代码
    git clone 远程仓库地址(内部已实现git remote add origin 远程仓库地址)
2. 切换分支
    git checkout 分支

在新电脑上进行开发
1. 切换到dev分支进行开发
    git checkout dev
2. 把master分支合并到dev(仅一次)
    git merge master
3. 修改代码
4. 提交代码
    git add .
    git commit -m 'xx'
    git push origin dev

回老电脑上继续写代码
1. 切换到dev分支进行开发
    git checkout dev
2. 拉代码
    git pull origin dev
3. 继续开发
4. 提交代码
    git add .
    git commit -m 'xx'
    git push origin dev
```

 开发完毕之后，在dev分支上commit，然后切换到master分支上合并一下dev分支，然后推送到GitHub上的master分支上，就算是完成了 。

 最后，因为我们dev分支上也是最新的代码了，也可以将我们的dev分支推送到GitHub上的dev分支上，其实推送之前有个好习惯就是在dev分支上合并一下master分支的代码git merge master，这里我没有写，但是个好习惯 。

 以后如果我们还想继续开发，我们可以将我们的dev和master分支上的最新的代码都拉下来进行继续开发：

```
git pull origin dev
git pull origin master
```

### 第六步： **如果在公司忘记提交代码，怎么搞？**

```
在公司开发的时候
1. 拉代码
    git pull origin dev
2. 继续开发

3. 提交代码
    git add .
    git commit -m 'xx'
注：忘记push了,没有推给GitHub

回到家继续开发
1. 拉代码，发现在公司写的代码忘记提交到GitHub上了
    git pull origin dev
2. 继续开发其他功能
    但是在家里写的功能有可能和你在公司开发的代码有些冲突(在同一行)
3. 把dev分支也推送到了远程
    git add .
    git commit -m 'xxx'
    git push origin dev

第二天到了公司继续写代码
1. 拉代码，把昨天晚上在家里写的其他功能的代码拉到本地（有合并、可能产生冲突）
    git pull origin dev
2. 如果有冲突,手动解决冲突(公司电脑上昨天忘记push的代码和昨日回到家后写的代码可能有些冲突)
3. 继续开发其他功能
4. 把dev分支也推送到远程
    git add .
    git commit -m 'xxxx'
    git push origin dev
```

其实git pull origin dev等价于下面两个指令：

```
git fetch origin dev   #将远程仓库dev分支的代码拉到本地git的版本库中，为了和本地dev分支做个区分，远程拉下来的dev分支会叫另外一个名字：origin/dev  
git merge origin/dev  # 合并远程拉取下来的dev分支的代码
```

看图：

![数据迁移](../images/git/%E5%85%AC%E5%8F%B8%E5%BF%98%E8%AE%B0%E6%8F%90%E4%BA%A4%E4%BB%A3%E7%A0%81%E6%80%8E%E4%B9%88%E6%90%9E.png)