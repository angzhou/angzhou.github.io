---
layout: post
title: pip使用pypi的国内镜像
tags: python pip
---

我们用pip安装Python软件库时有时会特别慢，这是因为从国内访问缺省的国外pypi服务器非常慢，幸亏国内有pypi镜像，访问这些镜像服务器就快得多了。

根据我们经验豆瓣和阿里的镜像速度、稳定性、更新频率算好的，地址分别是：

    http://pypi.douban.com/simple
    http://mirrors.aliyun.com/pypi/simple

用pip时可以直接使用这些镜像，比如安装selenium，版本2.52.0:

    pip3 install selenium==2.52.0 -i http://pypi.douban.com/simple
    pip3 install selenium==2.52.0 -i http://mirrors.aliyun.com/pypi/simple

如果你pip版本较新，访问非https必须加trusted-host：

    pip3 install selenium==2.52.0 -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
    pip3 install selenium==2.52.0 -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com

如果要让pip以后都用国内镜像，而省去每次打那么多字，可以修改pip配置文件：

    mkdir -p ~/.pip
    vi ~/.pip/pip.conf  # vi可用emacs, pico其他文本编辑器代替

文本内容:

```sh
[global]
index-url=http://pypi.douban.com/simple
trusted-host=pypi.douban.com
```

或者用阿里:

```
[global]
index-url=http://mirrors.aliyun.com/pypi/simple
trusted-host=mirrors.aliyun.com
```

如果你需要更高的速度和稳定性，你可以选择自己建Pypi镜像，请看相关文章。
