---
layout: post
title:  "[转载]Linux下文本编码转换"
date:   2013-03-06 23:12:59
categories: tech forward
---

本文转载自ncforest[《Linux下文本编码转换》](http://blog.163.com/ncforest/blog/static/2956266420100604751946)

Linux下打开以前从 Windows 所保留下来的文件时可能出现乱码，比如带有中文注释的matlab文件。主要由于编码而造成的。可以用`iconv`命令来解决编码问题。

`iconv`是一个文件编码转换工具，其基本用法为:

{% highlight bash %}
iconv -f 原始编码 -t 目的编码 要转换的文件 -o 已转换的文件
{% endhighlight %}

举个例子，假如我们要将`sample.txt`文件从 gb2312 转换为 utf-8 编码，并输出为`converted.txt`文件，可以这样执行命令:

{% highlight bash %}
iconv -f gb2312 -t utf-8 sample.txt -o converted.txtde
{% endhighlight %}

如果想知道`iconv`支持转换哪些编码，则可以使用`-l`选项查看:

{% highlight bash %}
iconv -lde
{% endhighlight %}

更有用的命令是`enconv`，它会自动判断文本编码和系统使用的编码，并把文本的编码转换为系统编码，而且可以批量转换。使用这个命令需要先安装`enca`

{% highlight bash %}
sudo aptitude install enca
enconv *.txt
enconv *.m
{% endhighlight %}
