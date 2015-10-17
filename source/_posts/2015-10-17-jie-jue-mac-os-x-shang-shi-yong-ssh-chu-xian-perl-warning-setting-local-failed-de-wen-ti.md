---
layout: post
title: "解决 Mac OS X 上使用 SSH 出现 Perl Warning: Setting local failed 的问题"
date: 2015-10-17 12:01:56 +0800
comments: true
categories: ["技术","Mac","SSH"]
---

在 Mac 上使用 SSH 连接远程服务器，运行 mongo 会出现报错：

    Failed global initialization: BadValue Invalid or no user locale set. Please ensure LANG and/or LC_* environment variables are set correctly.

运行 mysql、psql 或者其他一些程序也可能出现如下警告：

    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
	    LANGUAGE = (unset),
	    LC_ALL = (unset),
	    LC_CTYPE = "UTF-8",
	    LANG = "en_US.UTF-8"
    are supported and installed on your system.
    perl: warning: Falling back to the standard locale ("C").

<!--more-->

这是因为 Mac OS X 使用 SSH 连接远程服务器的时候缺少两个变量，解决办法是在 Mac OS X 本机的 ~/.bash_profit 文件中添加下面两行代码：

    export LC_CTYPE=en_US.UTF-8
    export LC_ALL=en_US.UTF-8

重启终端（Terminal）后可以解决该问题。