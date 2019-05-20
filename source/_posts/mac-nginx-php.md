---
title: Mac上搭建Nginx+PHP开发环境
date: 2017-06-20 14:39:34
tags:
    - mac
    - nginx
    - php
---

### 安装homebrew

homebrew是mac下非常好用的包管理器，会自动安装相关的依赖包，将你从繁琐的软件依赖安装中解放出来。

1. brew安装程序的过程中需要用到苹果的xcode中的编译器，命令安装：

    ```
    xcode-select --install
    ```


2. 安装homebrew也非常简单，只要在终端中输入:

    ```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

    homebrew的常用命令:
    brew update # 更新可安装包的最新信息，建议每次安装前都运行下
    brew search pkg_name # 搜索相关的包信息
    brew install pkg_name # 安装包
    ```

### 安装nginx

1. 使用brew安装nginx，brew 执行完后，nginx就安装好了。

    ```
    brew search nginx # 搜索nginx安装包
    brew install nginx # 安装nginx


    # nginx 帮助信息：

    nginx version: nginx/1.10.2
    Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

    选项:
        -?,-h           : 打开帮助信息
        -v              : 显示版本信息并退出
        -V              : 显示版本和配置选项信息，然后退出
        -t              : 检测配置文件是否有语法错误，然后退出
        -q              : 在检测配置文件期间屏蔽非错误信息
        -s signal       : 给一个 nginx 主进程发送信号：stop（停止）, quit（退出）, reopen（重启）, reload（重新加载配置文件）
        -p prefix       : 设置前缀路径（默认是：/usr/local/Cellar/nginx/1.10.2_1/）
        -c filename     : 设置配置文件（默认是：/usr/local/etc/nginx/nginx.conf）
        -g directives   : 设置配置文件外的全局指令
    ```

2. 运行 nginx，默认的访问端口 8080，如果要改为常用的 80 端口，则要修改 "/usr/local/etc/nginx/nginx.conf" 下监听(listen)端口值。默认的文件访问目录(root)是 "/usr/local/Cellar/nginx/1.10.2_1/html"（这里的1.10.2_1是安装的 nginx 的版本，文件夹名以安装的 nginx 版本为准）。

3. 把 nginx 设置为开机启动运行：

    ```
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/nginx/1.10.2_1/homebrew.mxcl.nginx.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
    ```

### 安装php

1. 使用brew安装对应php版本

    ```
    brew install homebrew/php/php56
    ```

2. 以下为brew安装php56的日志，根据日志信息修改相应配置

    ```
    The php.ini file can be found in:
        /usr/local/etc/php/5.6/php.ini

    ✩✩✩✩ Extensions ✩✩✩✩

    If you are having issues with custom extension compiling, ensure that you are using the brew version, by placing /usr/local/bin before /usr/sbin in your PATH:

          PATH="/usr/local/bin:$PATH"

    PHP56 Extensions will always be compiled against this PHP. Please install them using --without-homebrew-php to enable compiling against system PHP.

    ✩✩✩✩ PHP CLI ✩✩✩✩

    If you wish to swap the PHP you use on the command line, you should add the following to ~/.bashrc, ~/.zshrc, ~/.profile or your shell's equivalent configuration file:
      export PATH="$(brew --prefix homebrew/php/php56)/bin:$PATH"

    ✩✩✩✩ FPM ✩✩✩✩

    To launch php-fpm on startup:
        mkdir -p ~/Library/LaunchAgents
        cp /usr/local/opt/php56/homebrew.mxcl.php56.plist ~/Library/LaunchAgents/
        launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php56.plist

    The control script is located at /usr/local/opt/php56/sbin/php56-fpm

    OS X 10.8 and newer come with php-fpm pre-installed, to ensure you are using the brew version you need to make sure /usr/local/sbin is before /usr/sbin in your PATH:

      PATH="/usr/local/sbin:$PATH"

    You may also need to edit the plist to use the correct "UserName".

    Please note that the plist was called 'homebrew-php.josegonzalez.php56.plist' in old versions of this formula.

    With the release of macOS Sierra the Apache module is now not built by default. If you want to build it on your system you have to install php with the --with-httpd24 option. See  brew options php56 for more details.

    To have launchd start homebrew/php/php56 now and restart at login:
      brew services start homebrew/php/php56
    ```

3. 启动php-fpm

    ```
    # 如果环境变量配置了PATH="/usr/local/sbin:$PATH"，直接执行
    sudo php56-fpm start
    # 若没有修改环境变量，执行
    sudo /usr/local/sbin/php56-fpm start
    ```

4. 设置 nginx 的 php-fpm 配置

    ```
    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    location ~ \.php$ {
        root           /Users/cafe/Sites;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
   ```
