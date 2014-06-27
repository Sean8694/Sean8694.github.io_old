---
layout: post
title: "CentOS 6.5 安装日记"
modified: 2014-06-27 22:10:44 +0800
tags: [CentOS]
---

入手了yixiao大神的“圣者遗物”，便随便先折腾着玩玩，安装了CentOS 6.5。在这里记录下安装和配置的记录，以便将来查阅。

###1. CentOS安装

这没啥说的，网上下载了CentOS的iso文件，写入U盘里。但是这次也遇到了一个问题，安装过程中报错，找不到文件。网上搜了一下，发现是因为iso文件也是有格式的，用某些软件将镜像写入U盘会使得文件扩展名丢失。无奈，用UltraISO重新将镜像写入U盘，重新安装成功。

安装前需要选择CentOS的版本，不同版本还是有很大区别的，网上查了一下记录下来:

|版本|说明|
|:--------|:-------:|
|Desktop:|基本的桌面系统，包括常用的桌面软件，如文档查看工具|
|Minimal:|基本的桌面系统，包含的软件更少|
|MinimalDesktop:|基本的系统，不包含任何可选的软件包|
|BasicServer:|基本的桌面系统，包含的软件更少|
|DatabaseServer:|基本系统平台，加上MySQL和PostgreSQL数据库的客户端，无桌面|
|WebServer:|基本系统平台，加上PHP,Webserver，还有MySQL和PostgreSQL数据库的客户端，无桌面|
|VirtualHost:|基本系统加虚拟化平台|
|SoftwareDevelopmentWorkstation:|包含的软泥吉安包较多，基本系统，虚拟化平台桌面环境，开发工具|
|----
|
{: rules="groups"}

这次装的是`WebServer`版本。

###2. 启用网卡

安装完成之后，当然是第一时间打开ssh-server，便再不用接键盘、显示器啥的外设了。CentOS安装完成之后发现网卡默认是不启用的，所以在打开网卡之前，还是不能拔掉外设。

配置网卡开机默认启用

{% highlight bash %}

$ vim /etc/sysconfig/network-scripts/ifcfg-eth0

{% endhighlight %}

可以看到

{% highlight bash %}

DEVICE=eth0
HWADDR=xx:xx:xx:xx:xx:xx
TYPE=Ethernet
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
ONBOOT=no
NM_CONTROLLED=yes
BOOTPROTO=dhcp

{% endhighlight %}

将其中的`ONBOOT=no`改为`ONBOOT=yes`即可。

###3. 配置IP

终于可以摆脱外设了，现在需要配置一下路由器，给这个机器配置一个静态IP，这样保证每次开机都能够找到它。









