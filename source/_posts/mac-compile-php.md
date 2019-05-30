---
title: Mac编译安装PHP5.6.40
tags:
  - php
date: 2019-05-30 21:58:42
---

### 起因
Mac iTerm下使用brew管理依赖可以说是不二选了，可是最新的brew upgrade已经从core中抛弃php 5.6了，唯一的办法可能就是自己编译安装，这一流坑算是要踩一遍了。

#### 开始
源码地址：`wget -c http://cn2.php.net/distributions/php-5.6.40.tar.gz`

解压文件：`tar -zxvf php-5.6.40.tar.gz`

#### 配置初始化
```bash
./configure \
--prefix=/usr/local/php5.6 \
--with-mysql \
--with-mysqli \
--with-pdo-mysql \
--with-mhash \
--with-curl \
--with-gd \
--with-zlib \
--with-mcrypt \
--with-xpm-dir=/usr/X11/include \
--with-gd=/usr/local/Cellar/gd/2.2.5 \
--with-freetype-dir=/usr/local/Cellar/freetype/2.10.0 \
--with-jpeg-dir=/usr/local/Cellar/libjpeg \
--with-png-dir=/usr/local/Cellar/libpng/1.6.37 \
--with-libxml-dir=/usr/local/Cellar/libxml2/2.9.9_2 \
--with-zlib-dir=/usr/local/Cellar/zlib/1.2.11 \
--with-iconv=/usr/local/Cellar/libiconv/1.16 \
--with-curl=/usr/local/Cellar/curl/7.65.0 \
--with-config-file-path=/usr/local/php5.6/etc \
--enable-gd-native-ttf \
--enable-gd-jis-conv \
--enable-xml \
--enable-mbstring \
--enable-sockets \
--enable-simplexml \
--enable-soap \
--enable-mbstring=all \
--enable-sockets \
--enable-pdo \
--enable-cli \
--enable-fpm
```
`./configure`可能会找不到相关依赖，比如gd库、zlib等等，不用急，基本上`brew search\install`都能搞定。

#### 编译安装

```
make && make install
```

#### 配置文件

```
# 复制php.ini
cp php.ini-development /usr/local/php5.6/etc/php.ini
# 复制php-fpn.conf
cp /usr/local/php5.6/etc/php-fpm.conf.default /usr/local/php5.6/etc/php-fpm.conf
```

修改`/usr/local/php5.6/etc/php-fpm.conf`设置`pid = run/php-fpm.pid`

#### php-fpm命令
- 启动：`/usr/local/php5.6/sbin/php-fpm`
- 重启：``kill -USR2 `cat /usr/local/php5.6/var/run/php-fpm.pid` ``
- 关闭：``kill -INT `cat /var/run/php-fpm/php-fpm.pid` ``

### 新增PHP扩展

以新增`mcrypt`扩展为例，进入对应php版本扩展目录
```
cd ~/Downloads/php-5.6.40/ext/mcrypt
```

编译安装扩展
```
/usr/local/php5.6/bin/php-config

./configure --with-php-config=/usr/local/php5.6/bin/php-config

make && make install
```

在`php.ini`添加一条`extension=mcrypt.so`

重启php-fpm


### 常见问题
问题：
```
configure: error: Please reinstall the libcurl distribution -
    easy.h should be in <curl-dir>/include/curl/
```
安装curl库`brew install curl`

问题：
```
configure: error: Please specify the install prefix of iconv with --with-iconv=<DIR>
```
安装libiconv`brew install libiconv`

问题：
```
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [libs/libphp5.bundle] Error 1
```
MakeFile 里面找到类似下面这一行：
```
EXTRA_LIBS = -lresolv -lmcrypt -lltdl -liconv-lm -lxml2 -lcurl -lssl -lcrypto
```
删除所有的 -lssl 和 -lcrypto 然后添加 libssl.dylib 和 libcrypto.dylib 的路径（如果你安装了 brew，那么则是 /usr/local/opt/openssl/lib/），重新运行 make 命令，done。


这里碰到了个问题，安装gd库依赖X11，也就是libxpm库，可是mac这个版本找不到，mac从lion版本已经抛弃并且继承在了XQuartz中了，我们需要下载dmg安装，最后把inclue目录引入进来。

X11代替方案：
下载安装 [https://dl.bintray.com/xquartz/downloads/XQuartz-2.7.11.dmg](https://dl.bintray.com/xquartz/downloads/XQuartz-2.7.11.dmg)

但是还是用问题，会报这个错误
```
/usr/local/src/php-5.6.40 /ext/gd/gd.c:57:22:   错误：X11/xpm.h：没有那个文件或目录
make: *** [ext/gd/gd.lo] 错误 1
```

我的分析是这样的，我们通过brew安装gd、x11等依赖，默认在Cellar下，但是没有放入compilers的环境变量下，所以想了个办法，我把X11目录中gd需要的头文件放到php的编译目录下，也就是如果你装了X11（XQuartz）,那么
```
cp -R /usr/X11/include/X11 php-5.6.40/ext/gd/
```