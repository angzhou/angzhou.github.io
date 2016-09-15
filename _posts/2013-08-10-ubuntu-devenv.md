---
layout: post
title: 新Ubuntu开发环境的准备
tags: ubuntusetup
---

<p class="message">你刚装了一台Ubuntu机器或者新买了一个Ubuntu云主机，第一步做什么?</p>

如果在中国，最好把apt的源换成国内的。

打开`/etc/apt/sources.list`，把里面的`us.archive.ubuntu.com`、`security.ubuntu.com`等换成`mirrors.aliyun.com`或`mirrors.163.com`等国内的镜像地址。有时候你在国内买云主机如阿里云的，那里面的源已经换过了，这一步是不需要的。

一般作为开发机，C和C++编译器是必不可少的，所以第一个安装的包是`build-essential`:

    sudo apt-get install build-essential

然后再按需要装一些常用的工具和库：

    sudo apt-get install git wget dpkg htop cloc tree lsof lynx sqlite3
    sudo apt-get install dnsutils whois unzip unrar
    sudo apt-get install language-pack-zh-hans language-pack-zh-hant
    sudo apt-get install ncurses-term software-properties-common
    sudo apt-get install cmake autogen autoconf
    sudo apt-get install libz-dev libssl-dev libxml2-dev
    sudo apt-get install libpng-dev libgif-dev libjpeg-dev libfreetype6-dev

为了方便阅读分了几行，这些都是可以一次安装的。 有些工具可能你暂时用不上，不过装了又没有坏处，以后说不定就用上了。
