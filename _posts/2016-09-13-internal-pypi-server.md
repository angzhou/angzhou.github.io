---
layout: post
title: 自建内部pypi镜像
tags: pip
---

如果你需要部署内部开发的python软件库，或者你的外网访问不稳定，你可以考虑自建pypi镜像。

## 安装pypiserver服务器

你的镜像服务器一定要和外网连接，要不然你无从下载软件库(除非你只提供内部开发的软件库，这情况是很少见的)。

选一个合适的位置，如你自己`~/pypiserver`下， 如果你要让镜像服务器运行在80端口，则需要以root运行，安装位置则可以选择例如`/opt/pypiserver`。

下面以机器IP(192.168.5.13)，用户目录下pypiserver为例，在venv环境安装pypiserver:

    cd
    mkdir -p pypiserver/packages
    cd pypiserver
    pyvenv .venv
    .venv/bin/pip install pypiserver

运行pypiserver:

    .venv/bin/pypi-server -p 8081 packages/

在其他机器的浏览器打开地址: `http://192.168.5.13:8081/simple`, 你会看到`Simple Index`字样，没有其他任何内容，这是因为你还没有任何package。

在服务器(192.168.5.13)上保持pypiserver继续运行，打开另一个terminal，下载packages:

先下载一个pytest最新版本：

    cd pypiserver
    pip install pytest -d packages

你需要什么库什么版本就下载什么版本，如SQLAlchemy 0.9.10和lxml 3.6.3:

    pip install sqlalchemy==0.9.10 lxml==3.6.3 -d packages

现在如果在浏览器上刷新`http://192.168.5.13:8081/simple`，你会发现上面有了packages的名字。

## 使用pypiserver服务器

现在你的pypiserver服务器可以作为一个pypi镜像使用了，就像pypi.douban.com/simple和mirrors.aliyun.com/pypi/simple一样，使用方法也一样。和公共镜像不同的是：公共镜像（豆瓣阿里等）有所有软件package的所有版本，而你的私有镜像只有你放上去的packages和版本。

如果你的用户机不能访问外网，你只能用内部私有镜像，则将你的`~/.pip/pip.conf`设为:

```sh
[global]
index-url=http://192.168.5.13:8081/simple
trusted-host=192.168.5.13
```

当你要安装的一个package你私有镜像里没有，它会自动转向官方服务器(http://pypi.python.org/simple)从那里下载。你如果希望转向国内的镜像服务器，则你启动pypiserver里要加fallback-url，如:

    .venv/bin/pypi-server --fullback-url http://pypi.douban.com/simple -p 8081 packages/

注意只要你的镜像里有一个package的版本，即使官方或者公共镜像上有其他版本，你pip客户也拿不到，比如：你镜像里lxml 只有3.6.3或者你有多个版本但最新版是3.6.3，你在客户机上安装：

    pip3 install lxml

就会获得3.6.3。就算你知道官方最新版是3.6.4，你也是无法安装的：

`pip install lxml  --upgrade` 和 `pip install lxml==3.6.4` 都不会成功。


但是如果你的用户机可以访问外网，你也可以在用户机加公共镜像以补充私有镜像，你的`~/.pip/pip.conf`设为:

```sh
[global]
index-url=http://192.168.5.13:8081/simple
extra-index-url=http://pypi.douban.com/simple
trusted-host=192.168.5.13 pypi.douban.com
```

这样，`pip install lxml  --upgrade` 或 `pip install lxml==3.6.4` 都会成功。
