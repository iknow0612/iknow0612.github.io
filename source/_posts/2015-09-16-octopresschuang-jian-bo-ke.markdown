---
layout: post
title: "使用Octopress创建博客, Google CDN被墙的解决办法"
date: 2015-09-16 16:51:14 +0800
comments: true
categories: [octopress, 技术]
---


官方文档提供 Octopress 创建博客的基本步骤：  
[http://octopress.org/docs/](http://octopress.org/docs/)

使用官方文档，创建过程还算比较顺利，很快就可以在 github 上运行起来，但是运行后，还是遇到了一个特别的问题：  
国内被墙后无法加载 google 提供的 jquery 文件和字体文件，导致 blog 打开速度超慢  
<!--more-->

## Google 被墙，无法加载指定 jquery 和字体
由于天朝无所不能的墙的存在，我们甚至上个 blog 都得“翻一翻”，对我们技术人员“翻一翻”到是好说，可是很多普通老百姓还是很难实现“翻一翻”的。所以，我们的 blog 是需要避免这种尴尬的情形的。  
当然，不仅仅是 blog，我们在使用一些国外优秀的前端页面时，也会遇到这种尴尬的情形。如果你发现你的页面需要很长的时间才能打开，那可能就是遇到了这个问题。
  
使用 Chrome 浏览器的 “Inspect Element”－“Network” 可以很方便的查看到被墙掉的文件：

{% img img-responsive /images/post/20150916/chrome_network_failed.png %}

可以看出来，被墙文件的共同特点是路径名中都包含 googleapis.com, 很显然 googleapis.com 已经被墙掉，内网无法访问。现在要做的就是把这些墙掉的文件替换掉。回到 octopress 目录，使用如下命令查找文档中包含 "googleapis.com" 的文件：

```
find ./ -name "*" | xargs grep "googleapis.com"
```

通过该命令，可以很方便的定位到文件夹中（包括子目录）哪些文档的内容包含某串字符。在这里，我们可以看到 public 和 source 文件夹中都有包含字符串 "googleapis.com" 的文件， public 中的文件是由 source 中的文件生成的，因此只需要更改 source 中的文件就可以。 

这里我们只需要更改两个文件

*source/_includes/head.html*
原文本

```
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
```
替换后的文本（替换为 baidu cdn)

```
<script src="//libs.baidu.com/jquery/1.9.1/jquery.min.js"></script>
```
*source/_includes/custom/head.html*

这里我直接将所有内容注释掉了，当然你也可以翻 墙后把 google 提供的字体下载到 github 目录中，将此处的路径改为你的地址。

重新生成并部署，就不会遇到这个问题了。

ps: _config.yml 中 twitter 开启时也存在 twitter 需要翻墙连接的情况，把 twitter 关闭后可以解决。
