---
title: Shoka如何使用自己的图床
date: 2024/5/30 21:00:25
comments: true
tags: 
- blog
- hexo
categories: 
- 计算机
- hexo
- shoka主题
---

# Shoka如何使用自己的图床

[首页](/)

# [#](#前言) 前言

在朋友推荐下我了解到 Shoka 主题，在尝试安装后发现网页无法正常随机图片

上网查询发现是原作者的某博图床接口被屏蔽了，便尝试更换为自己图床

关于我的图床图片比较模糊的是采用了大佬的图床图片，大佬经过了裁切处理

大佬链接：[关于 Shoka 图床又挂了这件事 - 博客 | Jiaying's Note = CWHISME = 人不能没有梦想，也要有足够的敬畏 (wangjiaying.top)](https://wangjiaying.top/2023/01/01/%E5%85%B3%E4%BA%8EShoka%E5%9B%BE%E5%BA%8A%E6%8C%82%E4%BA%86%E8%BF%99%E4%BB%B6%E4%BA%8B/ "关于 Shoka 图床又挂了这件事 - 博客 | Jiaying's Note = CWHISME = 人不能没有梦想，也要有足够的敬畏 (wangjiaying.top)")

# [#](#创建github图床) 创建 Github 图床

在这里我使用的是 PicGo 上传图片到 Github

关于使用与配置方法请参考官方文档

PicGo 官网：[https://picgo.github.io/PicGo-Doc/zh/](https://picgo.github.io/PicGo-Doc/zh/ "https://picgo.github.io/PicGo-Doc/zh/")

如果想快速上手可以参考下方链接

[使用 Github+picGo 搭建图床，保姆级教程来了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/489236769 "使用 Github+picGo 搭建图床，保姆级教程来了 - 知乎 (zhihu.com)")

# [#](#shoka配置图床) Shoka 配置图床

**Step1.**

在 `<root>\themes\shoka\scripts\helpers\engine.js` 打开找到代码

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token keyword">var</span> <span class="token function-variable function">parseImage</span> <span class="token operator">=</span> <span class="token keyword">function</span><span class="token punctuation">(</span><span class="token parameter">img<span class="token punctuation">,</span> size</span><span class="token punctuation">)</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="2"></td><td><pre>    <span class="token keyword">if</span> <span class="token punctuation">(</span>img<span class="token punctuation">.</span><span class="token function">startsWith</span><span class="token punctuation">(</span><span class="token string">'//'</span><span class="token punctuation">)</span> <span class="token operator">||</span> img<span class="token punctuation">.</span><span class="token function">startsWith</span><span class="token punctuation">(</span><span class="token string">'http'</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="3"></td><td><pre>      <span class="token keyword">return</span> img</pre></td></tr><tr><td data-num="4"></td><td><pre>    <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="5"></td><td><pre>        <span class="token keyword">return</span><span class="token string">'https://tva'</span><span class="token operator">+</span>randomServer<span class="token operator">+</span><span class="token string">'.sinaimg.cn/'</span><span class="token operator">+</span>size<span class="token operator">+</span><span class="token string">'/'</span><span class="token operator">+</span>img</pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token punctuation">}</span></pre></td></tr><tr><td data-num="7"></td><td><pre>  <span class="token punctuation">}</span></pre></td></tr></tbody></table>

将代码中这一部分更换为自己的图床链接，更改时建议删除后面的 size

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token string">'https://tva'</span><span class="token operator">+</span>randomServer<span class="token operator">+</span><span class="token string">'.sinaimg.cn/'</span><span class="token comment">// 更改为自己的链接</span></pre></td></tr></tbody></table>

> 图床链接前面可以加上 `图缓存链接` 让图片快速加载

链接：[https://images.weserv.nl/?url=](https://images.weserv.nl/?url= "https://images.weserv.nl/?url=")

> 具体为

(图缓存链接) + [https://raw.githubusercontent.com/](https://raw.githubusercontent.com/ "https://raw.githubusercontent.com/") 用户名 / 仓库名 /main（自己修改的分支名，默认为 main)/

> 如果嫌麻烦可以使用我的图床链接

链接：https://images.weserv.nl/?url=https://raw.githubusercontent.com/2670321532/blogImg/main/img/

更改后就会像这样

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token keyword">var</span> <span class="token function-variable function">parseImage</span> <span class="token operator">=</span> <span class="token keyword">function</span><span class="token punctuation">(</span><span class="token parameter">img<span class="token punctuation">,</span> size</span><span class="token punctuation">)</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="2"></td><td><pre>    <span class="token keyword">if</span> <span class="token punctuation">(</span>img<span class="token punctuation">.</span><span class="token function">startsWith</span><span class="token punctuation">(</span><span class="token string">'//'</span><span class="token punctuation">)</span> <span class="token operator">||</span> img<span class="token punctuation">.</span><span class="token function">startsWith</span><span class="token punctuation">(</span><span class="token string">'http'</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="3"></td><td><pre>      <span class="token keyword">return</span> img</pre></td></tr><tr><td data-num="4"></td><td><pre>    <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="5"></td><td><pre>       <span class="token keyword">return</span> <span class="token string">'https://images.weserv.nl/?url=https://raw.githubusercontent.com/FuFan1025/blog-img/master/'</span><span class="token operator">+</span>img</pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token punctuation">}</span></pre></td></tr><tr><td data-num="7"></td><td><pre>  <span class="token punctuation">}</span></pre></td></tr></tbody></table>

**Step2**

打开 PicGo, 将相册第二个模式设置为 `URL` 全选然后点击复制

创建一个.txt 文件，将复制的内容黏贴进去

里面会是一堆网页链接加数字，我们只需要后面的数字名称

例如

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token literal-property property">https</span><span class="token operator">:</span><span class="token operator">/</span><span class="token operator">/</span>cdn<span class="token punctuation">.</span>jsdelivr<span class="token punctuation">.</span>net<span class="token operator">/</span>gh<span class="token operator">/</span>FuFan1025<span class="token operator">/</span>blog<span class="token operator">-</span>img<span class="token operator">/</span>0cb1bc5a66e556f1c3328772117051b9<span class="token punctuation">.</span>jpg</pre></td></tr></tbody></table>

将前面的链接复制，ctrl+F 全选替换为 - + 空格，(别忘了空格) 变成

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token operator">-</span> 0cb1bc5a66e556f1c3328772117051b9<span class="token punctuation">.</span>jpg</pre></td></tr></tbody></table>

本方法参考链接：[关于 Shoka 图床修复 | Yiqiu Note = ほしづきよ = 始于两千年 (ui123456ax.github.io)](https://ui123456ax.github.io/2023/06/23/02_Shoka%E5%9B%BE%E5%BA%8A%E4%BF%AE%E5%A4%8D/ "关于 Shoka 图床修复 | Yiqiu Note = ほしづきよ = 始于两千年 (ui123456ax.github.io)") 可以查看更多方法

\# 本文作者： Fufan @FuFan's blog
\# 本文链接： https://fufan1025.github.io/2024/03/07/Shoka如何使用自己的图床/
\# 版权声明： 本站所有文章除特别声明外，均采用 (CC)BY-NC-SA 许可协议。转载请注明出处！