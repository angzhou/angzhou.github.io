---
layout: post
title: 升级gcc和g++
tags: ubuntusetup
---

先看你现有的gcc版本：

    gcc --version

如果你满意，则不需要升级，一般也真没必要升级。也许你要编译一个软件需要升级的gcc，或者你想试试一些c++14的新特性，你可以按以下步骤升级:

### 添加ppa

    sudo add-apt-repository ppa:ubuntu-toolchain-r/test  # 按回车
    sudo apt-get update

### 去掉现有的缺省

    sudo update-alternatives --remove-all gcc

### 安装新版gcc和g++

以下版本选一个或多个安装, 一般就选一个

    sudo apt-get install gcc-4.9 g++-4.9
    sudo apt-get install gcc-5 g++-5
    sudo apt-get install gcc-6 g++-6

### 设置缺省

下面执行一个或多个.

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 20 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 60 --slave /usr/bin/g++ g++ /usr/bin/g++-5
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 80 --slave /usr/bin/g++ g++ /usr/bin/g++-6

如果上面执行了多于一次:

    sudo update-alternatives --config gcc  # 选择一个回车

以后如果要切换gcc缺省版本，就再执行:

    sudo update-alternatives --config gcc

### 确认

    gcc --version
    g++ --version
