---
layout: post
title: "git 创建远程仓库"
modified: 2014-01-12 00:16:58 +0800
tags: [git]
---

##1. 登陆远程服务器，创建一个空仓库

登陆到服务器，运行:

{% highlight bash %}

$ mkdir hello.git 
$ cd hello.git 
$ git --bare init 

{% endhighlight %}

便可以生成新的git仓库

##2. 本地同步仓库

### 方法一: 比较简单的方法

直接拉取远程仓库:

{% highlight bash %}

$ git clone ssh://user@10.2.43.99/~/hello.git

{% endhighlight %}

此时会拉取下来一个空白仓库，hello文件夹下只有.git文件夹，并且无log信息。

本地进行添加文件和修改，创建commit，之后push到远程，以创建第一个节点:

{% highlight bash %}

$ touch readme.txt
$ git add .
$ git commit -m "init repo."
$ git push origin master

{% endhighlight %}

即可完成本地库与远程库同步

### 方法二: 比较麻烦的方法

在本地建立一个git库并初始化:

{% highlight bash %}

$ mkdir hello 
$ git init

{% endhighlight %}

在本地仓库做修改，再将本地仓库同步到远程仓库上

{% highlight bash %}

$ git remote add origin ssh://user@10.2.43.99/home/username/hello.git
$ git push origin master

{% endhighlight %}

> 以上方法二选一即可

> ##Reference:
>
> [git--远程仓库](http://blog.csdn.net/adream307/article/details/6394981)
>
> [为git安装一个远程仓库](http://zhiwei.li/text/2010/05/%E4%B8%BAgit%E5%AE%89%E8%A3%85%E4%B8%80%E4%B8%AA%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93/)
>
> [git 使用远程库 （zz）](http://www.cnblogs.com/dqshll/articles/1791234.html)


