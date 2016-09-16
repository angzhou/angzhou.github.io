---
layout: post
title: 安装PHP7和Apache
tags: ubuntusetup php
---

<p class="message">
怎样在Ubuntu上安装PHP7，和选装Apache
</p>

首先你系统应该已经有`add-apt-repository`命令，如果没有，则:

    sudo apt-get install software-properties-common
    sudo apt-get install python-software-properties

上面两个根据Ubuntu版本不同可能一个会失败，不过只要运行了总会得到`add-apt-repository`。


## PHP7

如果你只要PHP7，比如你只需命令行php-cli，或者你用nginx，只需要PHP-fpm后台:

    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update

然后选装PHP7组件，如：

    sudo apt-get install php7.0-cli php7.0-fpm
    php -v  # 测试

你也可以试试PHP7.1，虽然现在(2016/9)它还是Beta，没有正式发布，但应该很快了:

    sudo apt-get install php7.1-cli php7.1-fpm

## PHP7和Apache

先把以前的apache卸载:

    sudo apt-get purge 'apache2*'

加ppa后安装:

    sudo add-apt-repository ppa:ondrej/apache2
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
    sudo apt-get install apache2
    sudo apt-get install libapache2-mod-php7.0

apache/php7就装好了。到`/var/www/html`下，新建文件info.php，内容为:

```php
<?php echo phpinfo();
```

在浏览器地址打入 `机器ip/info.php` 就会看到php配置信息。

一般我们都要加装一些常用的PHP包：

    sudo apt-get install php7.0-mysql php7.0-mbstring php7.0-gd php7.0-xml

重启apapche：`sudo service apache2 restart`，重新加载info.php页会发现这些包都装好了。
