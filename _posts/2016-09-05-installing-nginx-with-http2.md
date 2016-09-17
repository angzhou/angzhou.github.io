---
layout: post
title: 安装支持http2的nginx
tags: nginx
---

### 下载安装openssl

一般系统自带的openssl版本比较老，不能支持HTTP2，我们需要自己下载编译openssl。

选取一个位置，到[openssl官网](https://www.openssl.org/source/)下载openssl源码，现在(2016/9)已经有1.1版本，但它刚出来，鉴于稳定性考虑，笔者还是推荐1.0版。

下面以用户下downloads目录为例，下载1.0.2h版openssl：

    cd ~/downloads
    wget https://www.openssl.org/source/openssl-1.0.2h.tar.gz
    tar xfz openssl-1.0.2h.tar.gz
    cd openssl-1.0.2h/
    ./config --prefix=/usr/local --openssldir=/usr/local/openssl
    make depend
    make
    sudo make install

### 下载安装nginx

去[nginx官网](http://nginx.org/en/download.html)下载源码，配置、编译、安装。

    cd ~/downloads
    wget http://nginx.org/download/nginx-1.11.4.tar.gz
    tar xfz nginx-1.11.4.tar.gz
    cd nginx-1.11.4/
    # 下面最好先 ./configure --help > my-configure
    # 然后编辑my-configure，配置好你需要的，再打`sh my-configure`执行
    # 这里就用一行命令行以清晰表达
    ./configure \
    --prefix=/usr/local \
    --conf-path=/usr/local/etc/nginx/nginx.conf \
    --user=nobody \
    --group=nogroup \
    --with-http_ssl_module \
    --with-openssl=/home/yourname/downloads/openssl-1.0.2h \
    --with-http_v2_module \
    --with-http_gzip_static_module \
    --with-http_auth_request_module  \
    --without-http_fastcgi_module \
    --without-http_uwsgi_module \
    --without-http_scgi_module \
    --without-http_memcached_module \
    --http-client-body-temp-path=/usr/local/tmp/http-client-body-temp \
    --http-proxy-temp-path=/usr/local/tmp/http-proxy-temp \
    --with-cc-opt='-DTCP_FASTOPEN=23'
    # 配置行结束
    make
    sudo make install

上面配置里可以加上你需要的nginx模块，比如：`--with-http_realip_module`, `--with-http_mp4_module`等。

安装后配置文件夹在`/usr/local/etc/nginx`下，配置nginx是另外话题，就不在这里写了。
