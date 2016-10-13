---
layout: post
title: 重设CPAN镜像
tags: perl
---

现在还是有人在用Perl的，比如我们。

用Perl多数时候都会用cpan，用cpan时一般是从一个cpan镜像下载软件包，有的cpan镜像有时好用有时不好用，没有能力让服务器持续在线的组织（比如：华中科技大学）最好不要做公告镜像，那不是帮人，是害人。

好在改变cpan所用镜像也很容易。

进入cpan:

    cpan

列出现在的镜像：

    o conf urllist

我的输出是：

    urllist
        0 [http://mirrors.hust.edu.cn/CPAN/]
        1 [http://ftp.neowiz.com/CPAN/]
        2 [http://cpan.sarang.net/]

华中科技大学的镜像排在第一个，但当时mirrors.hust.edu.cn是宕掉的。

把第一个shift掉:

    o conf urlist shift

再确认：

    o conf commit

就行了。

如果要加镜像:

    o conf urllist push http://cpan.sarang.net/

push也可用unshift代替，push是加到后面，unshift是加到前面。
