---
title: SourceTree配置代码冲突解决工具BeyondCompare
date: 2017-08-07 15:16:21
tags:
    - git
---

> 前提你已经安装SourceTree和BeyondCompare软件

1. 打开SourceTree->偏好设置(preference)->Diff

    ![](/img/sourcetree_compare.jpg)

    需要输入的命令如下:

    ```
    比较命令: /usr/bin/bcomp
    参数: $LOCAL $REMOTE
    合并命令: /usr/bin/bcomp
    参数: $LOCAL $REMOTE $BASE $MERGED
    ```

2. 打开终端 输入如下指令:

    ```
    sudo ln -s /Applications/Beyond\ Compare.app/Contents/MacOS/bcomp /usr/bin/
    ```

3. 在SourceTree中右键点击冲突文件，选择”启用外部合并工具”。




