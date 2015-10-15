---
layout: post
title: "Ubuntu 14.04 使用 cron 实现计划任务"
date: 2015-10-15 21:37:47 +0800
comments: true
categories: ["技术","linux","cron"] 
---


![](/images/post/20151015/Timing.jpg)

Windows 自带定时执行任务的工具叫做“计划任务”，Linux 下我们使用 Cron 实现这一功能。

<!--more-->

## 启动 cron 服务

通常 ubuntu 下自带 cron，如果没有也可以通过以下命令进行安装：

    $ apt-get install cron
    
若已经安装，输入以下命令判断 cron 服务是否启动：

    $ pgrep cron

如果有 pid （一串数字）输出则说明 cron 服务已经启动，没有任何输出说明需要手动启动 cron 服务。  
启动 cron 服务：

    $ service cron start

## 管理任务计划文件

cron 的所有任务计划都记录在 crontab 任务计划文件中，通过 crontab 命令对该任务文件进行管理。

    usage: crontab [-u user] file
           crontab [ -u user] [ -i ] { -e | -l | -r }
                   (default operation is replace, per 1003.2)
           -e      (edit user's crontab)
           -l      (list user's crontab)
           -r      (delete user's crontab)
           -i      (prompt before deleting user's crontab)

参数说明：

参数|说明
---|---
-u user|指定用户
-e|编辑某个用户的计划任务文件，若不指定用户，默认编辑当前用户的计划任务文件
-l|显示某个用户的计划任务文件，若不指定用户，默认显示当前用户的计划任务文件
-r|删除某个用户的计划任务文件，若不指定用户，默认删除当前用户的计划任务文件
-i|在删除之前推送确认提示

<br>
使用示例：

    $ crontab -u foo -e     #编辑用户 foo 的计划任务文件
    
    $ crontab -e            #编辑当前用户的计划任务文件
    
    $ crontab -u foo -l     #显示用户 foo 的计划任务文件
    
    $ crontab -l            #显示当前用户的计划任务文件
    
    $ crontab -r            #删除当前用户的计划任务文件

## 编辑任务计划文件

初次使用 crontab -e，可能需要选择编辑器，输入编辑器序号点击回车后进入计划任务文件编辑模式。若直接进入编辑模式忽略以上内容。

进入编辑模式后，按照指定格式添加任务计划。

任务计划的语法格式如下：

    m h dom mon dow   command
    0-59 0-23 1-31 1-12 0-7  command

* m: 表示分钟
* h: 表示小时
* dom: 表示日期
* mon: 表示月份
* dow: 表示星期
* command: 预执行的命令

另外需要使用一些特殊符号实现灵活的配置：

* \* 代表所有值
* / 代表“每”
* \- 代表范围
* , 分割数字

任务示例如下：

    ## 指定具体执行时间
    2   *  *  *  * ls    #每个小时的第2分钟执行一次 ls 命令
    30  7  *  *  * ls    #每天7：30执行一次 ls 命令
    30 20  *  *  2 ls    #每周二，20：30执行一次 ls 命令（0和7表示星期天）
    
    ## 指定间隔时间
    */2 *  *  *  * ls    #每隔2分钟执行一次 ls 命令
    
    ## 指定时间段
    30  7 3-6 *  * ls    #每个月的3，4，5，6号的7：30分各执行一次 ls 命令
    
    ## 指定多个时间
    30  7 3,6 *  * ls    #每月的3号和6号的7：30分各执行一次 ls 命令 

另外，使用 run-parts 可以运行指定目录下所有的脚本（注意脚本必须加上 "#!/bin/bash"，否则 run-parts 会调用失败）.

    30 7 * * * run-parts /home   #每天7：30运行 /home 目录下的所有脚本

按照指定格式添加任务，保存后，任务生效。

下面是一个实际的计划任务文件，包含系统自带注释和一个每两分钟执行一次输出字符串 ”Hello World“ 到 /home 目录下 cron_test 文件的计划任务。

    # Edit this file to introduce tasks to be run by cron. 
    # 
    # Each task to run has to be defined through a single line
    # indicating with different fields when the task will be run
    # and what command to run for the task
    # 
    # To define the time you can provide concrete values for
    # minute (m), hour (h), day of month (dom), month (mon),
    # and day of week (dow) or use '*' in these fields (for 'any').# 
    # Notice that tasks will be started based on the cron's system
    # daemon's notion of time and timezones.
    # 
    # Output of the crontab jobs (including errors) is sent through
    # email to the user the crontab file belongs to (unless redirected).
    # 
    # For example, you can run a backup of all your user accounts
    # at 5 a.m every week with:
    # 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
    # 
    # For more information see the manual pages of crontab(5) and cron(8)
    # 
    # m h  dom mon dow   command
    */2 * * * * echo "Hello World" >> /home/cron_test                                                          
