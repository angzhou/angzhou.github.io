---
layout: post
title: 安装Bugzilla
tags: ubuntusetup
---

Bugzilla是个很老的bug跟踪系统，但现在还是可以用的。

## 准备

我们用Apache cgi运行Buzilla，这样速度和效率会降低，但我们用户量并不多，这样安装配置都很简单，也足够用。

    sudo apt-get install apache2 libgd-dev libyaml-perl

libgd-dev是可选的。

Bugzilla是Perl写的，一般系统里都有Perl，所以不需另外安装。

## 下载解压

到[Bugzilla官网](https://www.bugzilla.org/download/)得到下载连接。

选择一个目录位置，比如/var/www，把bugzilla加压到这里。

    wget https://ftp.mozilla.org/pub/mozilla.org/webtools/bugzilla-5.0.3.tar.gz
    tar xfz bugzilla-5.0.3.tar.gz -C /var/www/

## 安装Bugzilla

下列操作最好用root用户进行。


    sudo su
    cd /var/www/bugzilla-5.0.3/
    vi localconfig

在localconfig里，把

    $webservergroup = 'apache';

改为：

    $webservergroup = 'www-data';

因为在Ubuntu上，apache的默认用户是www-data。

再把：

    $db_driver = 'mysql';

改为：

    $db_driver = 'sqlite';

如果用户量不多，sqlite数据库就足够了，也省得我们再去配置MySQL了。


运行：

    ./checksetup.pl
    ./install-module.pl DateTime
    ./install-module.pl DateTime::TimeZone
    ./install-module.pl GD

再运行`./checksetup.pl`，看还有什么需要安装的用`./install-module.pl`安装。


## 配置Apache

还是做为root用户

    cd /etc/apache2/mods-enabled/
    ln -s ../mods-available/cgi.load .
    ln -s ../mods-available/rewrite.load .
    vi /etc/apache2/sites-enabled/000-default.conf

在`/etc/apache2/sites-enabled/000-default.conf`的VirtualHost里，加入:

	DocumentRoot /var/www/bugzilla-5.0.3

    <Directory />
        Options FollowSymLinks
        AllowOverride None
    </Directory>

    <Directory /var/www/bugzilla-5.0.3/>
        AddHandler cgi-script .cgi
        Options +ExecCGI
        DirectoryIndex index.cgi index.html
        AllowOverride Limit FileInfo Indexes Options AuthConfig
    </Directory>

重启Apache：

    service apache2 restart

如果有错误就去/var/log/apache2里看error.log。
