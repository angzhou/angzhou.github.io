---
layout: post
title: 升级 MySQL 到 5.7
tags: mysql
---

MySQL 5.7 有了JSON数据类型，对我们很有用，但Ubuntu 12.04和14.04上是5.5和5.6，所以我们决定升级。

## 卸载MySQL旧版本

```bash
    sudo apt-get remove mysql-client-5.5 mysql-server-5.5  # 如果是5.6就把5.5换成5.6
    sudo apt-get autoremove
```

## 准备apt

到 [这里](https://dev.mysql.com/downloads/repo/apt/) 点击"Download"按钮，进去后拷贝`No thanks, just start my download.`连接:

    https://dev.mysql.com/get/mysql-apt-config_0.8.0-1_all.deb

可能最新的版本会不一样。

下载这个文件：

    wget https://dev.mysql.com/get/mysql-apt-config_0.8.0-1_all.deb

加入apt源：

    sudo dpkg -i mysql-apt-config_0.8.0-1_all.deb

运行时会出现选项，让mysql 5.7高亮，回车即可。

## 安装 MySQL 5.7

```bash
    sudo apt-get update
    sudo apt-get install mysql-server-5.7
```

## 升级原有数据库


```bash
    sudo mysql_upgrade -u root -p --force
```
