---
layout:     post
title:      timeshift的filter用法
subtitle:   
date:       2021-10-21
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
---


![timeshift-filter sample](img/post-timeshift-filter.png)

如图，表示备份`/home/zz`和`/root`下的隐藏文件及文件夹，但是不备份`/home/zz/.gradle/`等文件夹下的内容。注意`- /home/zz/.gradle/***`等必须在`+ /home/zz/.**`之上。
