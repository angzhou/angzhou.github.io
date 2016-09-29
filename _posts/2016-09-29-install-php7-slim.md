---
layout: post
title: 编译安装PHP7和Slim
tags: php
---

<p class="message">
以前说过怎样从ppa安装php7，这次是怎样编译源码并安装PHP7和Slim框架
</p>

##首先一些准备工作

在命令行:

    sudo apt-get install libcurl4-openssl-dev libgmp-dev libmhash-dev libmcrypt-dev
    sudo ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

这个连接也许不用做，在Ubuntu14.04上，gmp头文件不在/usr/include/gmp.h以至于configure找不到。

##下载解压configure、编译安装

下载解压：

    wget http://cn2.php.net/get/php-7.0.11.tar.xz/from/this/mirror -O php.tar.xz
    tar xfJ php.tar.xz 
    cd php-7.0.11/

configure:

    ./configure --prefix=/usr/local \
    --enable-fpm \
    --with-fpm-user=nobody \
    --with-fpm-group=nogroup \
    --with-config-file-path=/usr/local/etc \
    --with-config-file-scan-dir=/usr/local/etc/php.d \
    --with-pcre-regex \
    --with-zlib \
    --with-openssl \
    --enable-bcmath \
    --with-curl \
    --with-gd \
    --enable-gd-native-ttf \
    --with-gmp \
    --with-mhash \
    --enable-mbstring \
    --with-mcrypt \
    --with-pdo-mysql \
    --with-readline \
    --with-openssl-dir \
    --with-xmlrpc \
    --with-xsl \
    --enable-zip \
    --enable-mysqlnd \
    --with-pear

如果用postgres, 可以加：

    --with-pgsql \
    --with-pdo-pgsql \

Fedora和Ubuntu16.04用Systemd，可以加：

    --with-fpm-systemd \

编译安装：

    make
    sudo make install

最好把最后`sudo make install`的输出保存下来，特别是`Installing shared extensions:`位置。

##安装composer

    wget https://getcomposer.org/installer -O composer-setup.php
    sudo php composer-setup.php
    sudo mv composer.phar /usr/local/bin/composer
    sudo chown -R $USER:$USER ~/.composer/


##安装使用Slim和slim-skeleton

    composer create-project slim/slim-skeleton myproj
    cd myproj/
    php -S 0.0.0.0:8003 -t public public/index.php

在浏览器上打开http://ip:8003，可以看到slim的初步页面

页面的源码在src/，渲染的模板是templates/index.phtml。
