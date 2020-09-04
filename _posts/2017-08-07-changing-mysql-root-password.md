---
layout: post
title: 更改MySQL的root的密码
tags: mysql
---

升级了MySQL, root的密码被升没了。

先把mysql关掉：

    sudo service mysql stop

在一个terminal里启动：

    sudo -u mysql mysqld --skip-grant-tables --skip-networking

在另一个terminal里运行mysql客户：

    mysql

在客户里:

    mysql> flush privileges;
    mysql> use mysql;
    mysql> update user set authentication_string=PASSWORD("新密码") where User='root';
    mysql> update user set plugin="mysql_native_password";
    mysql> flush privileges;
    mysql> quit

这样就行了。

找到mysql服务器pid把它kill掉，重新启动:

    sudo service mysql start
