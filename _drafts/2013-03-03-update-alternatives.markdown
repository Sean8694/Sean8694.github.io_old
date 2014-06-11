---
layout: post
title:  "[转载]系统管理：update-alternatives"
date:   2013-03-03 22:33:48
categories: tech forward
---

本文转载自[CSDN](http://blog.csdn.net/dbigbear/article/details/4398961)

Linux 发展到今天，可用的软件已经非常多了。这样自然会有一些软件的功能大致上相同。例如，同样是编辑器，就有 `nvi`、`vim`、`emacs`、`nano`，而且我说的这些还只是一部分。大多数情况下，这样的功能相似的软件都是同时安装在系统里的，可以用它们的名称来执行。例如，要执行 `vim`，只要在终端下输入 `vim` 并按回车就可以了。不过，有些情况下我们需要用一个相对固定的命令调用这些程序中的一个。例如，当我们写一个脚本程序时，
只要写下 editor，而不希望要为“编辑器是哪个”而操心。Debian 提供了一种机制来解决这个问题，而 `update-alternatives` 就是用来实现这种机制的。

在说明 update-alternatives 的详细内容之间，先让我们看看系统中已有的例子。打开终端，执行下面的命令：
代码:

{% highlight bash %}
herbert@natsu:~$ ls -l /usr/bin/editor
lrwxrwxrwx  1 root root 24 2004-09-26 08:48 /usr/bin/editor -> /etc/alternatives/editor
herbert@natsu:~$ ls -l /etc/alternatives/editor
lrwxrwxrwx  1 root root 12 2004-10-27 16:24 /etc/alternatives/editor -> /usr/bin/vim
herbert@natsu:~$
{% endhighlight %}

我们看到，`editor` 这个可执行命令实际上是个符号链接，它指向 `/etc/alternatives/editor`；而 `/etc/alternatives/editor` 也是个符号链接，它指向 `/usr/bin/vim`。这样，当我输入 `editor` 并回车时，将执行 `vim`。之所以要在 `/usr/bin` 和 `/etc/alternatives` 中费心建立这样两个链接，就是要实现上面说到的特性：方便脚本程序的编写和系统的管理。

下面我们就来看看 `update-alternatives` 的功能。当然，如果你觉得我说得不详细，可以看看这个命令的 `manpage：UPDATE-ALTERNATIVES(8)`。

首先要介绍的参数是 `--display`。它使我们可以看到一个命令的所有可选命令。执行代码:
{% highlight bash %}
natsu:/home/herbert# update-alternatives --display editor
editor - status is auto.
 link currently points to /usr/bin/vim
/bin/ed - priority -100
 slave editor.1.gz: /usr/share/man/man1/ed.1.gz
/usr/bin/nvi - priority 19
 slave editor.1.gz: /usr/share/man/man1/nvi.1.gz
/bin/nano - priority 40
 slave editor.1.gz: /usr/share/man/man1/nano.1.gz
/usr/bin/vim - priority 120
 slave editor.1.gz: /usr/share/man/man1/vim.1.gz
/usr/bin/emacs21 - priority 0
 slave editor.1.gz: /usr/share/man/man1/emacs.1emacs21.gz
Current `best' version is /usr/bin/vim.
natsu:/home/herbert#
{% endhighlight %}

你可以看到我的机器上的所有可以用来被 `editor` 链接的命令。

下面说说 `--config`。这个选项使我们可以选择其中一个命令：
代码:
{% highlight bash %}
natsu:/home/herbert# update-alternatives --config editor

There are 5 alternatives which provide 'editor'.

  Selection    Alternative

-----------------------------------------------

      1        /bin/ed

      2        /usr/bin/nvi

      3        /bin/nano

*+    4        /usr/bin/vim

      5        /usr/bin/emacs21



Press enter to keep the default[*], or type selection number: 4

Using '/usr/bin/vim' to provide 'editor'.

natsu:/home/herbert#
{% endhighlight %}

我并没有修改它，因为我还是比较喜欢 `vim` 的。 当然，你可以选择别的程序。

说到这里我们就要介绍一些概念了。首先，update-alternatives 在一般情况下是由`postinst` 和 `prerm` 这样的安装脚本自动调用的，所以一个 `alternative` 的状态有两种：自动和手动。每个 `alternative` 的初始状态都是自动。如果系统发现管理员手动修改了一个 `alternative`，它的状态就从自动变成了手动，这样安装脚本就不会更新它了。如果你希望将一个 `alternative` 变回自动，只要执行
代码:
{% highlight bash %}
update-alternatives --auto editor
{% endhighlight %}

就可以了。你注意到了吗？我们说到了“名字”。该怎样写名字呢？这就是我们要介绍的第二个概念：
引用:
{% highlight bash %}
general name -- 这是指一系列功能相似的程序的“公用”名字（包括绝对路径），比如 /usr/bin/editor。
link -- 这是指一个 alternative 在 /etc/alternative 中的名字，比如 editor。
alternative -- 顾名思义，这是指一个可选的程序所在的路径（包括绝对路径），比如 /usr/bin/vim。
--auto，--display 和 --config 跟的都是 link。我们要说的第三个概念是优先级。这个比较简单，当然优先级越高的程序越好啦（在大多数情况下，我不想争论） 最后一个概念是主和从的 alternative。想想看，你将 /usr/bin/editor 链接到了 vim，可是当你执行 man editor 时看到的却是 emacs 的 manpage，你会做何感想呢？这就引出了主和从 alternative 的概念了：当更新主的 alternative 时，从的 alternative 也会被更新。
{% endhighlight %}

说完这四个重要的概念后，我们介绍另外两个选项。至于其他的。。。。我相信你会去看手册页的，对吗？ 

第一个是 --install。它的格式是：
代码:
{% highlight bash %}
update-alternatives --install gen link alt pri [--slave sgen slink salt] ...
{% endhighlight %}

gen，link，alt，pri 分别是我们上面说过的。如果需要从的 alternative，你可以用 --slave 加在后面。如果你在向一个已经存在的 alternative 组中添加新的 alternatives，该命令会把这些 alternatives 加入到这个已经存在的 alternative 组的列表中，并用新的可选命令作为新的命令；否则，将会建立一个新的自动的 alternative 组。

呜呼！我加入了一个错误的 alternative。我不想要这个 alternative 了。在这种情况下，可以执行下面的命令：
代码:
{% highlight bash %}
update-alternatives --remove name path
{% endhighlight %}

name 是一个在 /etc/alternatives 中的名字，也就是上面的 link，而 path 是希望删除的可选程序名的绝对路径名（放心，这样只是从列表中删除了这个程序，并不会真的从硬盘上删除程序的可执行文件）。如果从一个 alternative 组中删除了一个正在被链接的程序并且这个组仍然没有变成空的，update-alternatives 会自动用一个具有其他优先级的可选程序代替原来的程序。如果这个组变成空的了，那么连这个 alternative 组都会被移除。如果删除的程序没有被链接，则只有有关这个程序的信息会被移除。

说个例子吧。我下载了 Eclipse，并且安装了 gcj 和 gij。可是我发现 GNU 的 java 工具还不足以运行 Eclipse。我只好到 Sun 公司的网页上下载了它的 java 工具 jdk。因为是自己安装的，我将它们安装在 /usr/local 上，以便将来重新安装 Linux 系统时这些程序仍然可以使用。于是我要做的就是用这个 jdk 中的 java 和 javac 来代替系统原来的。执行代码:

{% highlight bash %}
natsu:/home/herbert# update-alternatives --display java

java - status is auto.

 link currently points to /usr/local/j2sdk1.4.2_06/bin/java

/usr/bin/gij-wrapper-3.3 - priority 33

 slave java.1.gz: /usr/share/man/man1/gij-wrapper-3.3.1.gz

/usr/local/j2sdk1.4.2_06/bin/java - priority 100

 slave java.1.gz: /usr/local/j2sdk1.4.2_06/man/man1/java.1

Current `best' version is /usr/local/j2sdk1.4.2_06/bin/java.

natsu:/home/herbert# update-alternatives --display javac

javac - status is auto.

 link currently points to /usr/local/j2sdk1.4.2_06/bin/javac

/usr/bin/gcj-wrapper-3.3 - priority 33

 slave javah: /usr/bin/gcjh-wrapper-3.3

 slave javac.1.gz: /usr/share/man/man1/gcj-wrapper-3.3.1.gz

 slave javah.1.gz: /usr/share/man/man1/gcjh-wrapper-3.3.1.gz

/usr/bin/gcj-wrapper-3.4 - priority 33

 slave javah: /usr/bin/gcjh-wrapper-3.4

 slave javac.1.gz: /usr/share/man/man1/gcj-wrapper-3.4.1.gz

 slave javah.1.gz: /usr/share/man/man1/gcjh-wrapper-3.4.1.gz

/usr/local/j2sdk1.4.2_06/bin/javac - priority 100

 slave javah: /usr/local/j2sdk1.4.2_06/bin/javah

 slave javac.1.gz: /usr/local/j2sdk1.4.2_06/man/man1/javac.1

 slave javah.1.gz: /usr/local/j2sdk1.4.2_06/man/man1/javah.1

Current `best' version is /usr/local/j2sdk1.4.2_06/bin/javac.

natsu:/home/herbert#
{% endhighlight %}

（你看到的是我更新以后的）就可以得到关于要更新哪些 alternatives 的信息。我是这么更新的：
代码:
{% highlight bash %}
update-alternatives --install /usr/bin/javac javac /usr/local/j2sdk1.4.2_06/bin/javac 100 --slave /usr/bin/javah javah /usr/local/j2sdk1.4.2_06/bin/javah --slave /usr/share/man/man1/javac.1.gz javac.1.gz /usr/local/j2sdk1.4.2_06/man/man1/javac.1 --slave /usr/share/man/man1/javah.1.gz javah.1.gz /usr/local/j2sdk1.4.2_06/man/man1/javah.1

update-alternatives --install /usr/bin/java java /usr/local/j2sdk1.4.2_06/bin/java 100 --slave /usr/share/man/man1/java.1.gz java.1.gz /usr/local/j2sdk1.4.2_06/man/man1/java.1
{% endhighlight %}
