---
layout: post
title: 安装TaskWarrior
tags: task
---

[TaskWarrior](https://taskwarrior.org/)是款命令行的任务管理软件。

### 准备

Ubuntu:

    sudo apt-get install pkg-config autoconf autogen cmake
    sudo apt-get install uuid-dev libgmp-dev libreadline-dev

Mac:

    brew install gnutls
    brew install cmake

### 装nettle>=3.1 (Mac不需要)

Ubuntun > 16.04 只需： `sudo apt install nettle-dev`

    wget https://ftp.gnu.org/gnu/nettle/nettle-3.1.tar.gz
    tar xfz nettle-3.1.tar.gz
    cd nettle-3.1/
    ./configure
    make
    sudo make install

### 装gnutls 3.5.4 (Mac不需要)

Ubuntun > 16.04 只需： `sudo apt install gnutls-bin`

    wget ftp://ftp.gnutls.org/gcrypt/gnutls/v3.5/gnutls-3.5.4.tar.xz
    tar xfJ gnutls-3.5.4.tar.xz
    cd gnutls-3.5.4/
    ./configure --with-included-libtasn1 --without-p11-kit
    make
    sudo make install


### 装taskwarrior 2.5.1

Ubuntun > 16.04 只需： `sudo apt install task`

    wget https://taskwarrior.org/download/task-2.5.1.tar.gz
    tar xzf task-2.5.1.tar.gz
    cd task-2.5.1
    cmake -DCMAKE_BUILD_TYPE=release .
    make
    sudo make install

### 配置taskwarrior

    export LD_LIBRARY_PATH=/usr/local/lib:/usr/lib  # 最好加到你的~/.bashrc里
    task version  # 打y后回车

### 使用

    task add write setting up server article due:tomorrow
    task add 给wendy买生日蛋糕 due:2016-10-18 project:GF priority:H
    task add 和jessica说分手 project:GF priority:L
    task add 给小明打电话 due:eom project:基友
    # 列出next tasks
    task
    # 只列出GF任务
    task project:GF

具体使用方法看[文档](https://taskwarrior.org/docs/)。

### 安装tasksh

    wget http://taskwarrior.org/download/tasksh-latest.tar.gz
    tar xfz tasksh-latest.tar.gz
    cd tasksh-1.1.0/
    cmake -DCMAKE_BUILD_TYPE=release .
    make
    sudo make install

