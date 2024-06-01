---
title: shoka基本配置
date: 2024/5/30 21:46:25
comments: true
tags: 
- blog
- hexo
categories: 
- 计算机
- hexo
- shoka主题配置
---



# Step.1 shoka基本配置

## [#](#站点别称) 站点别称

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">alternate</span><span class="token punctuation">:</span> Yume Shoka</pre></td></tr></tbody></table>

这里设置的名称代替 Logo，显示在页面顶部，以及页尾©️处

## [#](#静态文件目录) 静态文件目录

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">statics</span><span class="token punctuation">:</span> / <span class="token comment">#//cdn.jsdelivr.net/gh/amehime/shoka@latest/</span></pre></td></tr></tbody></table>

默认值是 `/` ，指使用本地静态文件  
可以修改成 `//cdn.jsdelivr.net/gh/您的github用户名/您的项目名@latest/` 这种形式，以使用 jsDelivr 进行加速。  
PS：jsDelivr 并不是实时更新，重新生成文件后需要耐心等待

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">css</span><span class="token punctuation">:</span> css</pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">js</span><span class="token punctuation">:</span> js</pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token key atrule">images</span><span class="token punctuation">:</span> images</pre></td></tr></tbody></table>

静态文件所处目录的实际目录名，这些一般不改。

## [#](#夜间模式) 夜间模式

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">darkmode</span><span class="token punctuation">:</span> <span class="token comment"># true</span></pre></td></tr></tbody></table>

默认情况下，是否开启夜间模式取决于（优先级从高到低）：

1.  访客点击页面头部切换按钮的自行选择
2.  访客切换了浏览设备的主题色调
3.  您的 `darkmode` 配置项

## [#](#自动定位) 自动定位

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">auto_scroll</span><span class="token punctuation">:</span> <span class="token comment"># false</span></pre></td></tr></tbody></table>

默认情况下，再次打开页面时，会自动滚动到上次浏览的位置。  
这个选项设为 `false` 时将停用此功能。

## [#](#加载动画) 加载动画

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token comment"># 是否显示页面加载动画 loading-cat</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">loader</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">start</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 当初次打开页面时，显示加载动画</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">switch</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># tab 切换到其他页面时，显示加载动画</span></pre></td></tr></tbody></table>

tab 切换后只是显示 loading 动画，实际并未重新加载页面

## [#](#页面特效) 页面特效

单击页面的烟花效果配置如下

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">fireworks</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">enable</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 是否启用</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">color</span><span class="token punctuation">:</span> <span class="token comment"># 烟花颜色</span></pre></td></tr><tr><td data-num="4"></td><td><pre>    <span class="token punctuation">-</span> <span class="token string">"rgba(255,182,185,.9)"</span></pre></td></tr><tr><td data-num="5"></td><td><pre>    <span class="token punctuation">-</span> <span class="token string">"rgba(250,227,217,.9)"</span></pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token punctuation">-</span> <span class="token string">"rgba(187,222,214,.9)"</span></pre></td></tr><tr><td data-num="7"></td><td><pre>    <span class="token punctuation">-</span> <span class="token string">"rgba(138,198,209,.9)"</span></pre></td></tr></tbody></table>

## [#](#加载谷歌字体) 加载谷歌字体

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">font</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">enable</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token comment"># Font options:</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token comment"># `external: true` will load this font family from `host` above.</span></pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token comment"># `family: Times New Roman`. Without any quotes.</span></pre></td></tr><tr><td data-num="6"></td><td><pre>  <span class="token comment"># `size: x.x`. Use `em` as unit. Default: 1 (16px)</span></pre></td></tr><tr><td data-num="7"></td><td><pre></pre></td></tr><tr><td data-num="8"></td><td><pre>  <span class="token comment"># Global font settings used for all elements inside &lt;body&gt;.</span></pre></td></tr><tr><td data-num="9"></td><td><pre>  <span class="token key atrule">global</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="10"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="11"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span> Mulish</pre></td></tr><tr><td data-num="12"></td><td><pre>    <span class="token key atrule">size</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="13"></td><td><pre></pre></td></tr><tr><td data-num="14"></td><td><pre>  <span class="token comment"># Font settings for alternate title.</span></pre></td></tr><tr><td data-num="15"></td><td><pre>  <span class="token key atrule">logo</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="16"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="17"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span> Fredericka the Great</pre></td></tr><tr><td data-num="18"></td><td><pre>    <span class="token key atrule">size</span><span class="token punctuation">:</span> <span class="token number">3.5</span></pre></td></tr><tr><td data-num="19"></td><td><pre></pre></td></tr><tr><td data-num="20"></td><td><pre>  <span class="token comment"># Font settings for site title.</span></pre></td></tr><tr><td data-num="21"></td><td><pre>  <span class="token key atrule">title</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="22"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="23"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span> Noto Serif JP</pre></td></tr><tr><td data-num="24"></td><td><pre>    <span class="token key atrule">size</span><span class="token punctuation">:</span> <span class="token number">2.5</span></pre></td></tr><tr><td data-num="25"></td><td><pre></pre></td></tr><tr><td data-num="26"></td><td><pre>  <span class="token comment"># Font settings for headlines (&lt;h1&gt; to &lt;h6&gt;).</span></pre></td></tr><tr><td data-num="27"></td><td><pre>  <span class="token key atrule">headings</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="28"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="29"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span> Noto Serif SC</pre></td></tr><tr><td data-num="30"></td><td><pre>    <span class="token key atrule">size</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="31"></td><td><pre></pre></td></tr><tr><td data-num="32"></td><td><pre>  <span class="token comment"># Font settings for posts.</span></pre></td></tr><tr><td data-num="33"></td><td><pre>  <span class="token key atrule">posts</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="34"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="35"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="36"></td><td><pre></pre></td></tr><tr><td data-num="37"></td><td><pre>  <span class="token comment"># Font settings for &lt;code&gt; and code blocks.</span></pre></td></tr><tr><td data-num="38"></td><td><pre>  <span class="token key atrule">codes</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="39"></td><td><pre>    <span class="token key atrule">external</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="40"></td><td><pre>    <span class="token key atrule">family</span><span class="token punctuation">:</span> Inconsolata</pre></td></tr></tbody></table>

此功能基本参考 NexT。  
加粗标题的字体总是使用 `Noto Serif` ，为了正确友好的显示日文中的汉字，会先后加载 `headings` 和 `title` 的字体设置。

## [#](#iconfont图标) `iconfont` 图标

主题没有直接使用 Font Awesome，是因为用不到那么多 icon 感觉非常浪费，因此在 Iconfont 上重新建立了一个项目。  
`font-family` 设为 `ic` ，所有字体样式前缀为 `i-` ，具体参见 `<root>/themes/shoka/source/css/_iconfont.styl` 。

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token comment"># project of https://www.iconfont.cn/</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token comment"># //at.alicdn.com/t/font_1832207_c8i9n1ulxlt.css =&gt; 1832207_c8i9n1ulxlt</span></pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token key atrule">iconfont</span><span class="token punctuation">:</span> <span class="token string">"1832207_c8i9n1ulxlt"</span></pre></td></tr></tbody></table>

如果需要添加或修改，请留言告诉我您的 [Iconfont](https://www.iconfont.cn/ " Iconfont") 用户名，我将把您添加到目前的[项目](https://www.iconfont.cn/manage/index?manage_type=myprojects&amp;projectId=1832207 "项目")中。

添加权限为 `只读` ，此后您可以任意全选，批量保存到购物车中，添加至您自己的项目里，并将主题配置文件中的 `iconfont` 值改为您的项目。

注意，您的项目应设置 `FontClass/Symbol 前缀` 为 `i-` 。

在 `<root>/source/_data/` 目录新建文件 `iconfont.styl` ，把新增或修改的图标样式复制到这个文件中。

> 自定义 `iconfont.styl` 文件将覆盖主题默认样式，为了避免出错，请保证原有样式名均存在，在原有样式基础上进行增删改。

## [#](#菜单与社交按钮) 菜单与社交按钮

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">menu</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">home</span><span class="token punctuation">:</span> / <span class="token punctuation">|</span><span class="token punctuation">|</span> home</pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">about</span><span class="token punctuation">:</span> /about/ <span class="token punctuation">|</span><span class="token punctuation">|</span> user</pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">posts</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="5"></td><td><pre>    <span class="token key atrule">default</span><span class="token punctuation">:</span> / <span class="token punctuation">|</span><span class="token punctuation">|</span> feather</pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token key atrule">archives</span><span class="token punctuation">:</span> /archives/ <span class="token punctuation">|</span><span class="token punctuation">|</span> list<span class="token punctuation">-</span>alt</pre></td></tr><tr><td data-num="7"></td><td><pre>    <span class="token key atrule">categories</span><span class="token punctuation">:</span> /categories/ <span class="token punctuation">|</span><span class="token punctuation">|</span> th</pre></td></tr><tr><td data-num="8"></td><td><pre>    <span class="token key atrule">tags</span><span class="token punctuation">:</span> /tags/ <span class="token punctuation">|</span><span class="token punctuation">|</span> tags</pre></td></tr><tr><td data-num="9"></td><td><pre>  <span class="token comment"># friends: /friends/ || heart</span></pre></td></tr><tr><td data-num="10"></td><td><pre>  <span class="token comment"># links: /links/ || magic</span></pre></td></tr><tr><td data-num="11"></td><td><pre></pre></td></tr><tr><td data-num="12"></td><td><pre><span class="token key atrule">social</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="13"></td><td><pre>  <span class="token key atrule">github</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//github.com/yourname <span class="token punctuation">|</span><span class="token punctuation">|</span> github <span class="token punctuation">|</span><span class="token punctuation">|</span> "<span class="token comment">#191717"</span></pre></td></tr><tr><td data-num="14"></td><td><pre>  <span class="token comment">#google: https://plus.google.com/yourname || google</span></pre></td></tr><tr><td data-num="15"></td><td><pre>  <span class="token key atrule">twitter</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//twitter.com/yourname <span class="token punctuation">|</span><span class="token punctuation">|</span> twitter <span class="token punctuation">|</span><span class="token punctuation">|</span> "<span class="token comment">#00aff0"</span></pre></td></tr><tr><td data-num="16"></td><td><pre>  <span class="token key atrule">zhihu</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//www.zhihu.com/people/yourname <span class="token punctuation">|</span><span class="token punctuation">|</span> zhihu <span class="token punctuation">|</span><span class="token punctuation">|</span> "<span class="token comment">#1e88e5"</span></pre></td></tr><tr><td data-num="17"></td><td><pre>  <span class="token key atrule">music</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/user/home?id=yourid || cloud-music || "#e60026"</span></pre></td></tr><tr><td data-num="18"></td><td><pre>  <span class="token key atrule">weibo</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//weibo.com/yourname <span class="token punctuation">|</span><span class="token punctuation">|</span> weibo <span class="token punctuation">|</span><span class="token punctuation">|</span> "<span class="token comment">#ea716e"</span></pre></td></tr><tr><td data-num="19"></td><td><pre>  <span class="token key atrule">about</span><span class="token punctuation">:</span> https<span class="token punctuation">:</span>//about.me/yourname <span class="token punctuation">|</span><span class="token punctuation">|</span> address<span class="token punctuation">-</span>card <span class="token punctuation">|</span><span class="token punctuation">|</span> "<span class="token comment">#3b5998"</span></pre></td></tr><tr><td data-num="20"></td><td><pre>  <span class="token comment">#email: mailto:yourname@mail.com || envelope || "#55acd5"</span></pre></td></tr><tr><td data-num="21"></td><td><pre>  <span class="token comment">#facebook: https://www.facebook.com/yourname || facebook</span></pre></td></tr><tr><td data-num="22"></td><td><pre>  <span class="token comment">#stackoverflow: https://stackoverflow.com/yourname || stack-overflow</span></pre></td></tr><tr><td data-num="23"></td><td><pre>  <span class="token comment">#youtube: https://youtube.com/yourname || youtube</span></pre></td></tr><tr><td data-num="24"></td><td><pre>  <span class="token comment">#instagram: https://instagram.com/yourname || instagram</span></pre></td></tr><tr><td data-num="25"></td><td><pre>  <span class="token comment">#skype: skype:yourname?call|chat || skype</span></pre></td></tr><tr><td data-num="26"></td><td><pre>  <span class="token comment">#douban: https://www.douban.com/people/yourname/ || douban</span></pre></td></tr></tbody></table>

如上，使用 `||` 作为分隔符，依次为 `链接 || 图标 || 颜色` 。  
注意，只需要写图标名称，如 `github` ，则会自动转换为 `ic i-github` 。  
十六进制颜色码需要 `""` 包绕。

`menu` 支持一级子目录，子目录设置中的第一项必须为 `default` ，用来定义父级按钮的样式。

菜单显示文字可以在语言包中定义，[具体请戳这里](../display/#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%AF%AD%E8%A8%80%E5%8C%85)

## [#](#边栏配置) 边栏配置

边栏可以选择在左侧，或右侧  
修改头像文件的地址，相对于静态文件目录 `images` 中配置的路径。

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">sidebar</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token comment"># Sidebar Position.</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">position</span><span class="token punctuation">:</span> left</pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token comment">#position: right</span></pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token comment"># Replace the default avatar image and set the url here.</span></pre></td></tr><tr><td data-num="6"></td><td><pre>  <span class="token key atrule">avatar</span><span class="token punctuation">:</span> avatar.jpg</pre></td></tr></tbody></table>

可以将自己的图片放在 `<root>/source/_data/images/` 目录，甚至以同名覆盖主题内默认的头像图片，[具体请戳这里](../display/#%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%BB%E9%A2%98%E5%9B%BE%E7%89%87)

## [#](#底部widgets) 底部 widgets

目前页面底部可以显示两个小部件，即 `随机文章` 和 `最近评论` 。

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">widgets</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">random_posts</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 显示随机文章</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">recent_comments</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 显示最近评论</span></pre></td></tr></tbody></table>

## [#](#字数及阅读时间统计) 字数及阅读时间统计

安装好 `hexo-symbols-count-time` 插件后，不需要修改站点配置文件，直接使用插件默认配置就行。

需要修改主题配置文件，找到两处 `cout` ，修改为 `true` ：

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token comment"># 页尾全站统计</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">footer</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">since</span><span class="token punctuation">:</span> <span class="token number">2010</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">count</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr><tr><td data-num="5"></td><td><pre></pre></td></tr><tr><td data-num="6"></td><td><pre><span class="token comment"># 文章界面统计</span></pre></td></tr><tr><td data-num="7"></td><td><pre><span class="token key atrule">post</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="8"></td><td><pre>  <span class="token key atrule">count</span><span class="token punctuation">:</span> <span class="token boolean important">true</span></pre></td></tr></tbody></table>

## [#](#文章评论) 文章评论

[如何获取 LeanCloud 的 appId 和 appKey](https://valine.js.org/quickstart.html "如何获取 LeanCloud 的 appId 和 appKey")。

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">valine</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">appId</span><span class="token punctuation">:</span> <span class="token comment">#Your_appId</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">appKey</span><span class="token punctuation">:</span> <span class="token comment">#Your_appkey</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">placeholder</span><span class="token punctuation">:</span> ヽ(○´∀`)ﾉ♪ <span class="token comment"># Comment box placeholder</span></pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token key atrule">avatar</span><span class="token punctuation">:</span> mp <span class="token comment"># Gravatar style : mp, identicon, monsterid, wavatar, robohash, retro</span></pre></td></tr><tr><td data-num="6"></td><td><pre>  <span class="token key atrule">pageSize</span><span class="token punctuation">:</span> <span class="token number">10</span> <span class="token comment"># Pagination size</span></pre></td></tr><tr><td data-num="7"></td><td><pre>  <span class="token key atrule">lang</span><span class="token punctuation">:</span> zh<span class="token punctuation">-</span>CN</pre></td></tr><tr><td data-num="8"></td><td><pre>  <span class="token key atrule">visitor</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 文章访问量统计</span></pre></td></tr><tr><td data-num="9"></td><td><pre>  <span class="token key atrule">NoRecordIP</span><span class="token punctuation">:</span> <span class="token boolean important">false</span> <span class="token comment"># 不记录 IP</span></pre></td></tr><tr><td data-num="10"></td><td><pre>  <span class="token key atrule">serverURLs</span><span class="token punctuation">:</span> <span class="token comment"># When the custom domain name is enabled, fill it in here (it will be detected automatically by default, no need to fill in)</span></pre></td></tr><tr><td data-num="11"></td><td><pre>  <span class="token key atrule">powerMode</span><span class="token punctuation">:</span> <span class="token boolean important">true</span> <span class="token comment"># 默认打开评论框输入特效</span></pre></td></tr><tr><td data-num="12"></td><td><pre>  <span class="token key atrule">tagMeta</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="13"></td><td><pre>    <span class="token key atrule">visitor</span><span class="token punctuation">:</span> 新朋友</pre></td></tr><tr><td data-num="14"></td><td><pre>    <span class="token key atrule">master</span><span class="token punctuation">:</span> 主人</pre></td></tr><tr><td data-num="15"></td><td><pre>    <span class="token key atrule">friend</span><span class="token punctuation">:</span> 小伙伴</pre></td></tr><tr><td data-num="16"></td><td><pre>    <span class="token key atrule">investor</span><span class="token punctuation">:</span> 金主粑粑</pre></td></tr><tr><td data-num="17"></td><td><pre>  <span class="token key atrule">tagColor</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="18"></td><td><pre>    <span class="token key atrule">master</span><span class="token punctuation">:</span> <span class="token string">"var(--color-orange)"</span></pre></td></tr><tr><td data-num="19"></td><td><pre>    <span class="token key atrule">friend</span><span class="token punctuation">:</span> <span class="token string">"var(--color-aqua)"</span></pre></td></tr><tr><td data-num="20"></td><td><pre>    <span class="token key atrule">investor</span><span class="token punctuation">:</span> <span class="token string">"var(--color-pink)"</span></pre></td></tr><tr><td data-num="21"></td><td><pre>  <span class="token key atrule">tagMember</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="22"></td><td><pre>    <span class="token key atrule">master</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="23"></td><td><pre>      <span class="token comment"># - hash of master@email.com</span></pre></td></tr><tr><td data-num="24"></td><td><pre>      <span class="token comment"># - hash of master2@email.com</span></pre></td></tr><tr><td data-num="25"></td><td><pre>    <span class="token key atrule">friend</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="26"></td><td><pre>      <span class="token comment"># - hash of friend@email.com</span></pre></td></tr><tr><td data-num="27"></td><td><pre>      <span class="token comment"># - hash of friend2@email.com</span></pre></td></tr><tr><td data-num="28"></td><td><pre>    <span class="token key atrule">investor</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="29"></td><td><pre>      <span class="token comment"># - hash of investor1@email.com</span></pre></td></tr></tbody></table>

tag 标签显示在评论者名字的后面，默认是 `tagMeta.visitor` 对应的值。  
在 `tagMeta` 和 `tagColor` 中，除了 `visitor` 这个 key 不能修改外，其他 key 都可以换一换，但需要保证一致性。

举个栗子

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">tagMeta</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>    <span class="token key atrule">visitor</span><span class="token punctuation">:</span> 游客</pre></td></tr><tr><td data-num="3"></td><td><pre>    <span class="token key atrule">admin</span><span class="token punctuation">:</span> 管理员</pre></td></tr><tr><td data-num="4"></td><td><pre>    <span class="token key atrule">waifu</span><span class="token punctuation">:</span> 我老婆</pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token key atrule">tagColor</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token key atrule">visitor</span><span class="token punctuation">:</span> <span class="token string">"#855194"</span></pre></td></tr><tr><td data-num="7"></td><td><pre>    <span class="token key atrule">admin</span><span class="token punctuation">:</span> <span class="token string">"#a77c59"</span></pre></td></tr><tr><td data-num="8"></td><td><pre>    <span class="token key atrule">waifu</span><span class="token punctuation">:</span> <span class="token string">"#ed6ea0"</span></pre></td></tr><tr><td data-num="9"></td><td><pre>  <span class="token key atrule">tagMember</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="10"></td><td><pre>    <span class="token key atrule">admin</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="11"></td><td><pre>      <span class="token comment"># - hash of admin@email.com</span></pre></td></tr><tr><td data-num="12"></td><td><pre>    <span class="token key atrule">waifu</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="13"></td><td><pre>      <span class="token comment"># - hash of waifu@email.com</span></pre></td></tr></tbody></table>

在文章 Front Matter 中也可以配置上述参数，访问该文章页面时，将覆盖全局配置。  
尤其可以用来配置一个特殊的 placeholder。

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">valine</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">placeholder</span><span class="token punctuation">:</span> <span class="token string">"1. 提问前请先仔细阅读本文档⚡\n2. 页面显示问题💥，请提供控制台截图📸或者您的测试网址\n3. 其他任何报错💣，请提供详细描述和截图📸，祝食用愉快💪"</span></pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token punctuation">---</span></pre></td></tr></tbody></table>

评论通知与管理工具建议使用这个 [Valine-Admin](https://github.com/DesertsP/Valine-Admin " Valine-Admin")。  
注意 `SITE_URL` 需要以 `/` 结尾。

如果某一篇文章需要关闭评论功能，则在文章 Front Matter 中配置：

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token punctuation">---</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">title</span><span class="token punctuation">:</span> 关闭评论</pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token key atrule">comment</span><span class="token punctuation">:</span> <span class="token boolean important">false</span></pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token punctuation">---</span></pre></td></tr></tbody></table>

## [#](#背景音乐) 背景音乐

在主题配置文件中，设置全局播放列表。  
在文章的 Front Matter 中，设置文章专有播放列表，访问该文章页面时，将覆盖全局配置。

单列表

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">audio</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/song<span class="token punctuation">?</span>id=1387098940</pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/playlist?id=2088001742</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//www.xiami.com/collect/250830668</pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//y.qq.com/n/yqq/playsquare/3535982902.html</pre></td></tr></tbody></table>

如上，可以直接使用网易云、虾米、QQ 音乐的播放列表、单曲，可以同时填写多个。

多列表

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">audio</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token punctuation">-</span> <span class="token key atrule">title</span><span class="token punctuation">:</span> 列表1</pre></td></tr><tr><td data-num="3"></td><td><pre>    <span class="token key atrule">list</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="4"></td><td><pre>      <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/playlist?id=2943811283</span></pre></td></tr><tr><td data-num="5"></td><td><pre>      <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/playlist?id=2297706586</span></pre></td></tr><tr><td data-num="6"></td><td><pre>  <span class="token punctuation">-</span> <span class="token key atrule">title</span><span class="token punctuation">:</span> 列表2</pre></td></tr><tr><td data-num="7"></td><td><pre>    <span class="token key atrule">list</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="8"></td><td><pre>      <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/playlist?id=2031842656</span></pre></td></tr></tbody></table>

如果需要自定义媒体文件，可以按照以下格式填写：

单列表

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">audio</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> <span class="token string">"曲目1"</span></pre></td></tr><tr><td data-num="3"></td><td><pre>    <span class="token key atrule">url</span><span class="token punctuation">:</span> <span class="token string">"播放地址"</span></pre></td></tr><tr><td data-num="4"></td><td><pre>    <span class="token key atrule">artist</span><span class="token punctuation">:</span> <span class="token string">"艺术家"</span></pre></td></tr><tr><td data-num="5"></td><td><pre>    <span class="token key atrule">cover</span><span class="token punctuation">:</span> <span class="token string">"封面"</span></pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token key atrule">lrc</span><span class="token punctuation">:</span> <span class="token string">"歌词"</span></pre></td></tr><tr><td data-num="7"></td><td><pre>  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> <span class="token string">"曲目2"</span></pre></td></tr><tr><td data-num="8"></td><td><pre>    <span class="token key atrule">url</span><span class="token punctuation">:</span> <span class="token string">"播放地址"</span></pre></td></tr><tr><td data-num="9"></td><td><pre>    <span class="token key atrule">artist</span><span class="token punctuation">:</span> <span class="token string">"艺术家"</span></pre></td></tr><tr><td data-num="10"></td><td><pre>    <span class="token key atrule">cover</span><span class="token punctuation">:</span> <span class="token string">"封面"</span></pre></td></tr><tr><td data-num="11"></td><td><pre>    <span class="token key atrule">lrc</span><span class="token punctuation">:</span> <span class="token string">"歌词"</span></pre></td></tr></tbody></table>

多列表

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">audio</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>    <span class="token punctuation">-</span> <span class="token key atrule">title</span><span class="token punctuation">:</span> 列表1</pre></td></tr><tr><td data-num="3"></td><td><pre>      <span class="token key atrule">list</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="4"></td><td><pre>        <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> <span class="token string">"曲目1"</span></pre></td></tr><tr><td data-num="5"></td><td><pre>          <span class="token key atrule">url</span><span class="token punctuation">:</span> <span class="token string">"播放地址"</span></pre></td></tr><tr><td data-num="6"></td><td><pre>          <span class="token key atrule">artist</span><span class="token punctuation">:</span> <span class="token string">"艺术家"</span></pre></td></tr><tr><td data-num="7"></td><td><pre>          <span class="token key atrule">cover</span><span class="token punctuation">:</span> <span class="token string">"封面"</span></pre></td></tr><tr><td data-num="8"></td><td><pre>          <span class="token key atrule">lrc</span><span class="token punctuation">:</span> <span class="token string">"歌词"</span></pre></td></tr><tr><td data-num="9"></td><td><pre>        <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> <span class="token string">"曲目2"</span></pre></td></tr><tr><td data-num="10"></td><td><pre>          <span class="token key atrule">url</span><span class="token punctuation">:</span> <span class="token string">"播放地址"</span></pre></td></tr><tr><td data-num="11"></td><td><pre>          <span class="token key atrule">artist</span><span class="token punctuation">:</span> <span class="token string">"艺术家"</span></pre></td></tr><tr><td data-num="12"></td><td><pre>          <span class="token key atrule">cover</span><span class="token punctuation">:</span> <span class="token string">"封面"</span></pre></td></tr><tr><td data-num="13"></td><td><pre>          <span class="token key atrule">lrc</span><span class="token punctuation">:</span> <span class="token string">"歌词"</span></pre></td></tr><tr><td data-num="14"></td><td><pre>    <span class="token punctuation">-</span> <span class="token key atrule">title</span><span class="token punctuation">:</span> 列表2</pre></td></tr><tr><td data-num="15"></td><td><pre>      <span class="token key atrule">list</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="16"></td><td><pre>        <span class="token punctuation">-</span> https<span class="token punctuation">:</span>//music.163.com/<span class="token comment">#/playlist?id=2031842656</span></pre></td></tr></tbody></table>

如果要关闭当前页面的背景音乐播放器，则在文章 Front Matter 中配置：

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token punctuation">---</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">title</span><span class="token punctuation">:</span> 关闭背景音乐</pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token key atrule">audio</span><span class="token punctuation">:</span> <span class="token boolean important">false</span></pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token punctuation">---</span></pre></td></tr></tbody></table>

## [#](#随机图库) 随机图库

+   默认的图片列表位于 `<root>/themes/shoka/_images.yml` 中。  
    使用了渣浪图库，使用一些上传工具，比如[这里](https://pic.gimhoy.com/ "这里")  
    上传后图片的链接是 `http://wx4.sinaimg.cn/large/6833939bly1gicmnywqgpj20zk0m8dwx.jpg` 。  
    只需要新一行写上 `- 6833939bly1gicmnywqgpj20zk0m8dwx.jpg` 。
    
    如果想要自定义，则在 `<root>/source/_data/` 目录新建一个 `images.yml` 文件，这个文件中的图片至少 6 枚，将完全覆盖默认的图片列表。
    
+   也可以直接在图片列表 yml 文件中，写上任意外链图片地址
    

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token punctuation">-</span> https<span class="token punctuation">:</span>//i.loli.net/2020/10/30/qAMYEFXxJcKRsiG.gif</pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token punctuation">-</span> https<span class="token punctuation">:</span>//i.loli.net/2020/10/30/rjdhcSgEN8COBPA.jpg</pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token punctuation">-</span> https<span class="token punctuation">:</span>//i.loli.net/2020/10/30/HKyzSd7NI3mlBpt.jpg</pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token punctuation">-</span> https<span class="token punctuation">:</span>//i.loli.net/2020/10/30/Y1CBXqgeokEs457.jpg</pre></td></tr><tr><td data-num="5"></td><td><pre><span class="token punctuation">-</span> https<span class="token punctuation">:</span>//i.loli.net/2020/10/30/Z5W6r2BSoiThHG1.jpg</pre></td></tr></tbody></table>

+   也可以在主题配置文件中，设置图床 API：

比如

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">image_server</span><span class="token punctuation">:</span> <span class="token string">"https://acg.xydwz.cn/api/api.php"</span></pre></td></tr></tbody></table>

## [#](#加载第三方组件) 加载第三方组件

包括

<table><tbody><tr><td><code>pace</code></td><td>加载进度条</td><td>全局</td></tr><tr><td><code>pjax</code></td><td>页面无刷新加载</td><td>全局</td></tr><tr><td><code>anime</code></td><td>js 动画效果</td><td>全局</td></tr><tr><td><code>algolia</code> <code>instantsearch</code></td><td>基于 algolia 的站内搜索</td><td>全局</td></tr><tr><td><code>lazyload</code></td><td>图片懒加载</td><td>全局</td></tr><tr><td><code>quicklink</code></td><td>链接资源预加载</td><td>全局</td></tr><tr><td><code>fetch</code></td><td>获取播放列表</td><td>全局</td></tr><tr><td><code>katex</code> <code>copy_tex</code></td><td>数学公式显示及复制</td><td>按需</td></tr><tr><td><code>fancybox</code></td><td>图片放大显示及排列</td><td>按需</td></tr><tr><td><code>valine</code></td><td>基于 LeanCloud 的评论系统及文章阅读次数统计</td><td>按需</td></tr><tr><td><code>chart</code></td><td>图表显示</td><td>按需</td></tr></tbody></table>

以上文件加载全部基于 jsDelivr，并对全局加载的组件进行了文件合并。  
如果不明白啥意思，则不要轻易修改。

主题版本升级的时候，可能会修改这里。  
如果修改过主题默认 `_config.yml` ，记得更新主题时，末尾的 `vendors` 也要及时修改。

[Hexo](/tags/Hexo/) [教程](/tags/%E6%95%99%E7%A8%8B/)

# Step.2 界面显示

## [#](#首页置顶文章) 首页置顶文章

在文章的 Front Matter 设置 `sticky: true` ，则该文章将显示在首页最上方的 `置顶文章` 列。  
多篇文章按照发布时间倒序排列，不分页。

<table><tbody><tr><td data-num="1"></td><td><pre>---</pre></td></tr><tr><td data-num="2"></td><td><pre>title: 置顶文章</pre></td></tr><tr><td data-num="3"></td><td><pre>sticky: true</pre></td></tr><tr><td data-num="4"></td><td><pre>---</pre></td></tr></tbody></table>

## [#](#首页精选分类) 首页精选分类

想要在首页显示分类翻转块，需要按照以下示例的方式，给需要显示的分类加上封面图。

1.  首先，修改站点配置：  
    找到 `category_map:` ，配置每个分类对应的英文映射，比如：
    
    <table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">category_map</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre>  <span class="token key atrule">计算机科学</span><span class="token punctuation">:</span> computer<span class="token punctuation">-</span>science</pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token key atrule">Java</span><span class="token punctuation">:</span> java</pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">C++</span><span class="token punctuation">:</span> cpp</pre></td></tr><tr><td data-num="5"></td><td><pre>  <span class="token key atrule">二进制杂谈</span><span class="token punctuation">:</span> note</pre></td></tr><tr><td data-num="6"></td><td><pre>  <span class="token key atrule">计算机程序设计（C++）-西安交通大学</span><span class="token punctuation">:</span> course<span class="token punctuation">-</span><span class="token number">1</span></pre></td></tr><tr><td data-num="7"></td><td><pre>  <span class="token key atrule">零基础学Java语言-浙江大学-翁恺</span><span class="token punctuation">:</span> course<span class="token punctuation">-</span><span class="token number">1</span></pre></td></tr><tr><td data-num="8"></td><td><pre>  <span class="token key atrule">面向对象程序设计-Java语言-浙江大学-翁恺</span><span class="token punctuation">:</span> course<span class="token punctuation">-</span><span class="token number">2</span></pre></td></tr></tbody></table>
    
    > 注意：hexo 会自动处理路径中的特殊字符，~\`!@#$%^&\*()-\_+={}|\\;:"'<>,.? 以及空格，这些全部会被替换成 `-`  
    > 所以避免在设置中使用上述字符，否则可导致无法抓取到目录下的 `cover.jpg`
    
2.  在 `<root>/source/_posts` 文件夹相应的目录里，存放封面图  
    例子：如 [第 1 周 计算](https://shoka.lostyu.me/computer-science/java/course-1/week-1/) 这篇文章。  
    所处的分类是
    
    <table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">categories</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token punctuation">-</span> <span class="token punctuation">[</span>计算机科学<span class="token punctuation">,</span> Java<span class="token punctuation">,</span> 零基础学Java语言<span class="token punctuation">-</span>浙江大学<span class="token punctuation">-</span>翁恺<span class="token punctuation">]</span></pre></td></tr></tbody></table>
    
    现在需要在首页显示 `零基础学Java语言-浙江大学-翁恺` 这个分类，翻转卡片后，显示这个分类下的文章们。  
    而该分类经过英文映射，它的路径将是 `/computer-science/java/course-1/` 。
    
    那么，请在 `<root>/source/_posts/computer-science/java/course-1/` 的目录下放置 `cover.jpg` 文件。  
    只要 `分类路径` 对应的目录下**存在** `cover.jpg` 文件，这个分类就会在首页显示。  
    在进行 `hexo g` 时，本分类的封面图会自动被复制到 public 目录里相应的位置。
    
3.  事实上，为了方便文章管理，这个分类下所有文章的 md 文件，我都会放在 `<root>/source/_posts/computer-science/java/course-1/` 这个目录下。
    
    且站点配置文件里，永久链接设置如下
    
    <table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">permalink</span><span class="token punctuation">:</span> <span class="token punctuation">:</span>title/</pre></td></tr></tbody></table>
    
    `hexo g` 后，文章的 html 文件们将全部生成到 `<root>/public/computer-science/java/course-1/` 目录。  
    具体可以查看[本博客的 github 仓库](https://github.com/amehime/shoka "本博客的 github 仓库")。
    
4.  文章详情界面中的 `系列文章` ，显示的是与当前文章同一分类的其他文章，并按照文章名正序排序。
    

> o (\*￣▽￣\*) ゞ  
> 其实，不设置 `category_map` 也可以，只要在分类路径对应的文件夹下存在 `cover.jpg` 文件就行。  
> 现在，这项功能与 `category_dir` 的配置也无关， `hexo g` 生成后，图片会自动被转移到 `category_dir` 的相关子目录下。

## [#](#文章封面图) 文章封面图

+   如果文章的 Front Matter 设置了 `cover: image path` ，则封面会显示这张图片。
    
    举个栗子
    
    <table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">title</span><span class="token punctuation">:</span> Images</pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">cover</span><span class="token punctuation">:</span> assets/wallpaper<span class="token punctuation">-</span>2572384.jpg</pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token comment"># 或者写成</span></pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token key atrule">cover</span><span class="token punctuation">:</span> http<span class="token punctuation">:</span>//placehold.it/350x150.jpg</pre></td></tr><tr><td data-num="5"></td><td><pre><span class="token punctuation">---</span></pre></td></tr></tbody></table>
    
    这里 `cover` 的值可以是位于 `source` 目录里的图片文件，此处是 `<root>/source/assets/wallpaper-2572384.jpg` 文件，也可以是一个某网址。
    
+   如果文章是一个 `gallery post` ，即 Front Matter 设置了 `photos` ，则会封面会显示设置的第一张图片。
    
    举个栗子
    
    <table><tbody><tr><td data-num="1"></td><td><pre><span class="token key atrule">title</span><span class="token punctuation">:</span> Gallery Post</pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">photos</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token punctuation">-</span> assets/wallpaper<span class="token punctuation">-</span>2572384.jpg</pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token punctuation">-</span> assets/wallpaper<span class="token punctuation">-</span>2311325.jpg</pre></td></tr><tr><td data-num="5"></td><td><pre><span class="token punctuation">-</span> assets/wallpaper<span class="token punctuation">-</span>878514.jpg</pre></td></tr><tr><td data-num="6"></td><td><pre><span class="token punctuation">-</span> http<span class="token punctuation">:</span>//placehold.it/350x150.jpg</pre></td></tr><tr><td data-num="7"></td><td><pre><span class="token punctuation">---</span></pre></td></tr></tbody></table>
    
    此时默认会显示第一个图片，即位于 `<root>/source/assets/` 目录里的 `wallpaper-2572384.jpg` 。
    
+   如果站点配置中设置了 `post_asset_folder: true` ，那么上述本地图片路径应为 `<root>/source/_posts/文章同名的文件夹/assets/wallpaper-2572384.jpg` ，当然此时 `assets` 目录可以省掉。
    
+   如果以上设置均不存在，将显示一张随机图片，[随机图库配置戳此](../config/#%E9%9A%8F%E6%9C%BA%E5%9B%BE%E5%BA%93)。
    

## [#](#图片展示与相册) 图片展示与相册

在文章的 Front Matter 设置 `fancybox: false` ，可以关闭文章页的图片展示功能。  
使用 [Justified-Gallery](http://miromannino.github.io/Justified-Gallery/ " Justified-Gallery") 对 Gallery Post 内图案进行排列。

下面介绍一些小技巧：

1.  让图案下方显示 `title` 的 markdown 代码

<table><tbody><tr><td data-num="1"></td><td><pre>![这里是 alt](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg "这里是 title")</pre></td></tr></tbody></table>

[这里是title](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg)

2.  设置图片的大小

<table><tbody><tr><td data-num="1"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg "定义图片大小 - 固定宽度和高度"){height="100px" width="400px"}</pre></td></tr><tr><td data-num="2"></td><td><pre></pre></td></tr><tr><td data-num="3"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg "定义图片大小 - 固定宽度"){width="400px"}</pre></td></tr><tr><td data-num="4"></td><td><pre></pre></td></tr><tr><td data-num="5"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg "定义图片大小 - 固定高度"){height="100px"}</pre></td></tr></tbody></table>

[定义图片大小-固定宽度和高度](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg)

[定义图片大小-固定宽度](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg)

[定义图片大小-固定高度](https://tva3.sinaimg.cn/large/6833939bly1gicis081o9j20zk0m8dmr.jpg)

3.  除了在 Front Matter 里配置 `photos` 可以显示相册图案列表外，还可以这样写

<table><tbody><tr><td data-num="1"></td><td><pre>## 图案列表 No.1</pre></td></tr><tr><td data-num="2"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1giclfdu6exj20zk0m87hw.jpg "这里是 title")</pre></td></tr><tr><td data-num="3"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1giclflwv2aj20zk0m84qp.jpg)</pre></td></tr><tr><td data-num="4"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1giclg5ms2rj20zk0m8u0x.jpg)</pre></td></tr><tr><td data-num="5"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1giclhnx9glj20zk0m8npd.jpg)</pre></td></tr><tr><td data-num="6"></td><td><pre>{.gallery}</pre></td></tr><tr><td data-num="7"></td><td><pre></pre></td></tr><tr><td data-num="8"></td><td><pre>## 图案列表 No.2</pre></td></tr><tr><td data-num="9"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gicitht3xtj20zk0m8k5v.jpg)</pre></td></tr><tr><td data-num="10"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1giclil3m4ej20zk0m8tn8.jpg)</pre></td></tr><tr><td data-num="11"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gicljgocqbj20zk0m8e81.jpg)</pre></td></tr><tr><td data-num="12"></td><td><pre>![](https://tva3.sinaimg.cn/large/6833939bly1gipetfk5zwj20zk0m8e81.jpg)</pre></td></tr><tr><td data-num="13"></td><td><pre>{.gallery data-height="120"}</pre></td></tr></tbody></table>

`data-height` 用来设置每行的高度，默认为 `220`

### [#](#图案列表no1) 图案列表 No.1

[这里是title](https://tva3.sinaimg.cn/large/6833939bly1giclfdu6exj20zk0m87hw.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1giclflwv2aj20zk0m84qp.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1giclg5ms2rj20zk0m8u0x.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1giclhnx9glj20zk0m8npd.jpg)

### [#](#图案列表no2) 图案列表 No.2

[](https://tva3.sinaimg.cn/large/6833939bly1gicitht3xtj20zk0m8k5v.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1giclil3m4ej20zk0m8tn8.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1gicljgocqbj20zk0m8e81.jpg)[](https://tva3.sinaimg.cn/large/6833939bly1gipetfk5zwj20zk0m8e81.jpg)

## [#](#自定义页面配色) 自定义页面配色

主题配色全部在 `<root>/themes/shoka/source/css/_colors.styl` 文件中，可以自行查看。

在 `<root>/source/_data/` 目录新建文件 `colors.styl` ，在此文件中添改样式。

> 自定义 `colors.styl` 文件将覆盖主题默认样式，为了避免出错，请保证原有样式名均存在，在原有样式基础上进行增删改。

主题支持在 `<root>/source/_data/` 目录建立三个自定义 `styl` 文件：

| 自定义文件名    | 对应默认样式文件 | 样式功能                                          |
| --------------- | ---------------- | ------------------------------------------------- |
| `colors.styl`   | `_colors.styl`   | 页面配色                                          |
| `iconfont.styl` | `_iconfont.styl` | [图标样式](../config/#iconfont%E5%9B%BE%E6%A0%87) |
| `custom.styl`   | \-               | 任意自定义样式                                    |

## [#](#自定义主题图片) 自定义主题图片

如果想要修改主题的 `<root>/themes/shoka/source/images/` 目录内的某张图片，请在 `<root>/source/_data/` 目录新建目录 `images` ，并在这个文件夹中添加同名文件，部署时将自动覆盖主题内的默认图片。

可以用此方法自定义头像、打赏二维码等图片，并且避免覆盖更新主题时遗失自定义文件。

## [#](#自定义语言包) 自定义语言包

本功能参考 NexT，主要可以用来定义菜单等处显示的文字，且可以方便主题无脑覆盖升级。

在 `<root>/source/_data/` 目录新建文件 `languages.yml` 。

按照以下格式添加配置项：

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token comment"># language</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token key atrule">zh-CN</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="3"></td><td><pre>  <span class="token comment"># items</span></pre></td></tr><tr><td data-num="4"></td><td><pre>  <span class="token key atrule">post</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="5"></td><td><pre>    <span class="token key atrule">copyright</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="6"></td><td><pre>      <span class="token comment"># the translation you perfer</span></pre></td></tr><tr><td data-num="7"></td><td><pre>      <span class="token key atrule">author</span><span class="token punctuation">:</span> 本文博主</pre></td></tr><tr><td data-num="8"></td><td><pre><span class="token key atrule">en</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="9"></td><td><pre>  <span class="token key atrule">menu</span><span class="token punctuation">:</span></pre></td></tr><tr><td data-num="10"></td><td><pre>    <span class="token key atrule">travellings</span><span class="token punctuation">:</span> Travellings</pre></td></tr></tbody></table>

可以参考主题目录下的 `example/source/_data` 文件夹。

> 站点配置及文件的 Front Matter 中， `language` 项只支持 `zh-CN` 、 `zh-HK` 、 `zh-TW` 、 `ja` 、 `en` 。  
> 类似写成 `zh_CN` 这样是不可以的。

+   **本文作者：** Ruri Shimotsuki **@**優萌初華
+   **本文链接：** [https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/display/](https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/display/ "Step.3 界面显示")
+   **版权声明：** 本站所有文章除特别声明外，均采用 [**(CC)**BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh "(CC)BY-NC-SA") 许可协议。转载请注明出处！