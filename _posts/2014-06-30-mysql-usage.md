---
layout: post
title: "MySQL 基础知识"
modified: 2014-06-30 19:58:02 +0800
tags: [MySQL]
---

##基础知识

###1. MySQL的数据类型

MySQL有三大类数据类型, 分别为数字、日期\时间、字符串, 这三大类中又更细致的划分了许多子类型:

* 数字类型
    * 整数: tinyint、smallint、mediumint、int、bigint
    * 浮点数: float、double、real、decimal
* 日期和时间: date、time、datetime、timestamp、year
* 字符串类型
    * 字符串: char、varchar
    * 文本: tinytext、text、mediumtext、longtext
    * 二进制(可用来存储图片、音乐等): tinyblob、blob、mediumblob、longblob

[MySQL数据类型](http://www.cnblogs.com/zbseoag/archive/2013/03/19/2970004.html)

##基本操作

###1. 连接数据库

1. 登录到MySQL

{% highlight bash %}

mysql -h 主机名 -u 用户名 -p

{% endhighlight %}

-h : 该命令用于指定客户端所要登录的MySQL主机名, 登录当前机器该参数可以省略;

-P : MySQL的主机端口;

-u : 所要登录的用户名;

-p : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

例如：

{% highlight bash %}

$ mysql -h127.0.0.1 -uroot -p
$ Enter password:

{% endhighlight %}

输入密码后变可以登录本机MySQL

----

###2. 数据库

* 创建一个数据库

使用 create database 语句可完成对数据库的创建, 创建命令的格式如下:

{% highlight bash %}

mysql> create database 数据库名 [其他选项];

{% endhighlight %}

例如我们需要创建一个名为 samp_db 的数据库, 在命令行下执行以下命令:

{% highlight bash %}

mysql> create database samp_db character set gbk;

{% endhighlight %}

* 显示数据库

使用命令可以显示出所有的已创建的数据库

{% highlight bash %}

mysql> show databases;

{% endhighlight %}

 命令查看已经创建了哪些数据库。

* 选择所要操作的数据库

要对一个数据库进行操作, 必须先选择该数据库, 否则会提示错误:

{% highlight bash %}

ERROR 1046(3D000): No database selected

{% endhighlight %}

两种方式对数据库进行使用的选择:

一: 在登录数据库时指定, 命令:

{% highlight bash %}

$ mysql -D 所选择的数据库名 -h 主机名 -u 用户名 -p

{% endhighlight %}

例如登录时选择刚刚创建的数据库: 

{% highlight bash %}

$ mysql -D samp_db -u root -p

{% endhighlight %}

二: 在登录后使用 use 语句指定, 命令:

{% highlight bash %}

mysql> use 数据库名;

{% endhighlight %}

use 语句可以不加分号, 执行`use samp_db`来选择刚刚创建的数据库, 选择成功后会提示: Database changed



