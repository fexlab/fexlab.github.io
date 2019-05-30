---
title: PHP之日志系统SeasLog安装
date: 2017-06-20 14:50:58
tags:
    - mac
    - php
cover: /img/cover_4.jpg
---

> An effective,fast,stable log extension for PHP.

1、下载[SeasLog](http://pecl.php.net/package/SeasLog) `wget -c https://github.com/SeasX/SeasLog/archive/SeasLog-1.6.9.tar.gz`

2、解压下载好的文件包 `tar -zxvf SeasLog-1.6.9.tar.gz`

3、切换到SeasLog-1.6.9目录执行phpize命令，如果phpize命令找不到，那就用绝对路径执行`/usr/local/php/bin/phpize`操作，目录中会多出一个configure文件（需要安装autoconf依赖）

4、检测SeasLog是否依赖别的包，在当前文件执行 `./configure --with-php-config=/usr/bin/php-config`

5、编译并安装 `make && make install`

6、在php.ini文件中配置SeasLog信息

```bash
[seaslog]
;configuration for php SeasLog module
extension = seaslog.so

;默认log根目录
seaslog.default_basepath = /data/logs

;默认logger目录
seaslog.default_logger = default

;日期格式配置 默认"Y-m-d H:i:s"
seaslog.default_datetime_format = "Y-m-d H:i:s"

;日志格式模板 默认"%T | %L | %P | %Q | %t | %M"
seaslog.default_template = "%T | %L | %P | %Q | %t | %M"

;是否以type分文件 1是 0否(默认)
seaslog.disting_type = 1

;是否每小时划分一个文件 1是 0否(默认)
seaslog.disting_by_hour = 0

;是否启用buffer 1是 0否(默认)
seaslog.use_buffer = 1

;buffer中缓冲数量 默认0(不使用buffer_size)
seaslog.buffer_size = 100

;记录日志级别，数字越大，根据级别记的日志越多。
;0-EMERGENCY 1-ALERT 2-CRITICAL 3-ERROR 4-WARNING 5-NOTICE 6-INFO 7-DEBUG 8-ALL
;默认8(所有日志)
;
;   注意, 该配置项自1.7.0版本开始有变动。
;   在1.7.0版本之前, 该值数字越小，根据级别记的日志越多:
;   0-all 1-debug 2-info 3-notice 4-warning 5-error 6-critical 7-alert 8-emergency
;   1.7.0 之前的版本，该值默认为0(所有日志);
seaslog.level = 0
```

> 如果安装后没有生效，检查PHP版本和SeasLog版本是否兼容。