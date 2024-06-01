---
title: hexo服务启动
date: 2023/7/8 13:46:25
comments: true
tags: 
- hexo
categories: 
- 计算机
- hexo
- 博客
---

```vbscript
Sub Main
	xsh.Screen.Synchronous = true
	xsh.Screen.Send("sudo -i" + "\n")
	xsh.Screen.WaitForString("Password: ")
	xsh.Screen.Send("Dwz110..." + "\n")
	xsh.Screen.WaitForString("root@dwz:~# ")
	xsh.Screen.Send("cd /volume1/blog_web/blog" + "\n")
	xsh.Screen.WaitForString("root@dwz:/volume1/blog_web/blog# ")
	xsh.Screen.Send("hexo server" + "\n")
End Sub
```

