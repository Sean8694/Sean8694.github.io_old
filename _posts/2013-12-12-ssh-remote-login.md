---
layout: post
title:  "SSH 使用公钥远程登录"
date:   2012-12-12 12:25:10
categories: ssh
tags: [ssh, linux]
---

ssh 可以使用ssh-keygen生成公钥/私钥对，实现无密码远程登录。以下是配置方法：

例如: 机器A(10.2.43.22)登录到机器B(10.2.43.99)

###1.  在机器A执行:

{% highlight bash %}

$ ssh-keygen

{% endhighlight %}

连续三个回车后，便可以在`~/.ssh`目录下生成公钥/私钥对

###2.  将`~/.ssh/id_rsa.pub`文件复制到机器B

{% highlight bash %}

$ scp ~/.ssh/id_rsa.pub user@10.2.43.99:~

{% endhighlight %}

###3.  登陆到机器B

{% highlight bash %}

$ ssh user@10.2.43.99

{% endhighlight %}

将`id_rsa.pub`文件内容追加到`authorized_keys`

{% highlight bash %}

$ cat id_rsa.pub >> ~/.ssh/authorized_keys

{% endhighlight %}

###4.  确认`~/.ssh/authorized_keys`文件的访问权限为**600**

{% highlight bash %}

$ chmod 600 ~/.ssh/authorized_keys

{% endhighlight %}

至此，则可以从机器A无密码登陆到机器B了。

###References:
[使用ssh公钥实现免密码登录](http://blog.sina.com.cn/s/blog_4077692e0100qjkp.html)
[使用ssh公钥实现ssh免密码登录](http://hi.baidu.com/meloidea/item/15d43d2dd11d010e72863eb4)
