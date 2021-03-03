---
layout:     post
title:      gdbserver调试Android native app
subtitle:   
date:       2021-03-03
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
    - Android
    - 编程
---

> gdbserver调试Android native app

## 前言
网上的教程用了都无法正常调试，记录下自己成功的方法。

## 准备
不要使用Android中自带的gdbserver，要自己push进去以与主机端NDK里的gdb配套（即使Android中的gdbserver与NDK中的gdb同版本也可能不兼容）。

将gdbserver与待测app分别push到Android中:

```shell
adb push /path/to/Android/SDK/ndk/21.1.6352462/prebuilt/android-arm64/gdbserver/gdbserver /data/z/tmp/
adb push /path/to/android/libs/arm64-v8a/* /data/z/tmp/
```

编译native app时使用 `ndk-build NDK_DEBUG=1` 编译带调试符号的bin文件

## 调试
1. 使用gdbserver执行native app，并绑定到1235端口。`LD_LIBRARY_PATH`是因为app需要依赖文件夹下的so，没有依赖可以去掉。此时app未启动，处于等待gdb连接状态。

    ```shell
    LD_LIBRARY_PATH=. ./gdbserver :1235 ./app
    ```

2. 映射Android的1235端口到主机端的1235端口

    ```shell
    adb forward tcp:1235 tcp:1235
    ```

3. 主机端运行与gdbserver同一个NDK下的主机的gdb

    ```shell
    /path/to/Android/SDK/ndk/21.1.6352462/prebuilt/linux-x86_64/bin/gdb
    ```

4. gdb中配置符号

    ```
    set solib-search-path /path/to/android/libs/arm64-v8a/
    file path/to3/android/libs/arm64-v8a/demo-api
    ```

5. gdb中连接gdbserver

    ```
    target remote 127.0.0.1:1235
    ```

6. gdb中启动app

    ```
    r
    ```
