---
layout: post
title: 安装TaskWarrior Server
tags: task
---

[TaskWarrior](https://taskwarrior.org/)是款命令行的任务管理软件。

taskd 是taskwarrior的同步服务器。

### 准备

参看[安装TaskWarrior](/2016/09/02/installing-taskwarrior/)先装taskwarrior(即客户端)。

安装必要软件包：

    sudo apt-get install uuid-dev libgmp-dev libreadline-dev

### 装taskd

TaskWarrior网站上的taskd下载此时(2016/9/3)是1.1.0，有些老，我们将用1.2.0：

    git clone https://git.tasktools.org/scm/tm/taskd.git
    cd taskd
    git checkout 1.2.0
    cd src
    git clone https://git.tasktools.org/scm/tm/libshared.git
    cd ..
    cmake -DCMAKE_BUILD_TYPE=release .
    make
    sudo make install

### 配置taskd

    mkdir ~/taskd_data
    export TASKDDATA=~/taskd_data
    taskd init

    # 现在仍在taskd目录下
    cd pki
    vi vars

把CN=localhost的localhost改成你的服务器域名或IP, 假设我们改成了your_server，

(其他的内容也可以酌情改)

    ./generate
    cp client.cert.pem $TASKDDATA
    cp client.key.pem  $TASKDDATA
    cp server.cert.pem $TASKDDATA
    cp server.key.pem  $TASKDDATA
    cp server.crl.pem  $TASKDDATA
    cp ca.cert.pem     $TASKDDATA
    taskd config --force client.cert $TASKDDATA/client.cert.pem
    taskd config --force client.key $TASKDDATA/client.key.pem
    taskd config --force server.cert $TASKDDATA/server.cert.pem
    taskd config --force server.key $TASKDDATA/server.key.pem
    taskd config --force server.crl $TASKDDATA/server.crl.pem
    taskd config --force ca.cert $TASKDDATA/ca.cert.pem
    cd $TASKDDATA
    taskd config --force log $PWD/taskd.log
    taskd config --force pid.file $PWD/taskd.pid
    taskd config --force server your_server:53589  # your_server和上面vars里CN一致

### 启动taskd

    taskdctl start
    taskdctl status


### 配置task用户, 指向本地服务器

    # 回到taskd/pki目录
    cd ~/taskd/pki
    taskd add org Public
    taskd add user 'Public' 'your_name'  # your_name是用户名

这里会产生一个`New user key`, 比如`94ba61f8-5065-43d7-8dac-7c02523b0133`

    ./generate.client your_name

会产生两个文件:`your_name.key.pem`和`your_name.cert.pem`, 将这两个文件和ca.cert.pem拷贝到~/.task里：

    cp your_name.key.pem ~/.task
    cp your_name.cert.pem ~/.task
    cp ca.cert.pem ~/.task
    task config taskd.certificate -- ~/.task/your_name.cert.pem
    task config taskd.key -- ~/.task/your_name.key.pem
    task config taskd.ca -- ~/.task/ca.cert.pem
    task config taskd.server  -- your_server:53589
    task config taskd.credentials -- Public/wensheng/new_user_key

your_server仍是vars里CN的域名或IP.

new_user_key就是`task add user`那步得到的字串.

### 开始同步任务

    task sync init
    task

如果没有错误，把`~/.task`目录和`~/.taskrc`打包:

    cd
    tar cfz task.tgz .task .taskrc

把task.tgz拷到其他机器上：

    scp task.tgz remote_ip:/home/your_name/

我们把装了taskd的机器叫机器1，其他的机器为机器2、机器3等, 机器2、3上应该已经安装了taskwarrior.

登录机器2，解压task.tgz:

    tar xfz task.tgz
    task

`task`会显示和机器1 `task`一样的任务列表。

回到机器1，添加一项新任务：

    task add 机器1上的新任务
    task sync

回到机器2，由于没有同步，`task`显示的还是原来的内容，只需运行：

    task sync

再打`task`，会看到`机器1上的新任务`已经被同步过来了。

同样的，在机器2上添加任务后sync, 回到机器1后sync，也能看到机器2的添加和修改。

机器2上的配置可以复制到机器3上。
