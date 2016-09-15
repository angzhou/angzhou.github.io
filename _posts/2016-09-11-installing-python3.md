---
layout: post
title: Ubuntu上装Python3
tags: ubuntusetup python
---

一般来讲，你可以直接:

```bash
sudo apt-get install python3
```
来安装Python3，然后再装我们经常用到的venv：

```bash
sudo apt-get install python3.4-venv`
```

新的Ubuntu如16.04里python缺省就是python3.5，所以什么也不用做。

但是如果你需要更新的Python版本，比如你要用[asyncio](https://docs.python.org/3/library/asyncio.html)，它只在3.5以后才有，你的ubuntu 14.04的python3是3.4，或者你想用[f字串](https://www.python.org/dev/peps/pep-0498/)，3.6才有，你的Ubuntu16.04的Python是3.5，或者你就是不喜欢Ubuntu提供的版本，这些情况你就只有自己装了。

## 步骤

* 准备

除了`build-essential libz-dev libssl-dev libxml2-dev`等一般编译所需要的包外，最好安装这些额外的包：

    sudo apt-get install liblzma-dev libbz2-dev libsqlite3-dev libreadline6-dev

* 下载源码：

去[官网](https://www.python.org/downloads/source/)下载你要的版本的源码，我们以3.6.0b1为例。

    wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0b1.tar.xz

* 编译

通常的解压、配置、编译

    tar xfJ Python-3.6.0b1.tar.xz 
    cd Python-3.6.0b1/
    ./configure
    make

* 安装

把可执行文件、库、头文件装到`/usr/local`下

    sudo make install

`/usr/local/bin`里python3，pip3，pyvenv这三个可执行文件就可供使用了。
