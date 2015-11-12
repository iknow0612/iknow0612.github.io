---
layout: post
title: "浏览器的 new Date('yyyy.mm.dd') 坑"
date: 2015-11-10 00:32:16 +0800
comments: true
categories: ["技术","Javascript"]
---


这个坑让我填了半个多小时。发个博文做纪念!

    new Date('yyyy.mm.dd')

以上这段简单的代码，在 Chrome 下可以完美运行，在 Safari 下运行却会返回无效日期(Invalid Date)。

为了保证兼容性，应该使用如下代码：

    new Date('yyyy-mm-dd')

ps: 不同浏览器引擎对 js 的实现都存在细微的差别，一定要小心这些陷阱。写代码要注意兼容性。
