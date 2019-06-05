---
layout:     post
title:      使用buildroot创建交叉编译工具链
subtitle:   
date:       2019-06-01
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
    - buildroot
    - 交叉编译
    - 编程
---

> 使用buildroot创建所需的交叉编译工具链（toolchain）

## 下载
在 <https://buildroot.org/download> 下载任意一个压缩包并解压

## 配置
buildroot配置起来和配置内核的界面及方法一样。  
buildroot预置了很多平台的配置文件（configs/下），可以在预置配置上修改，也可以直接修改。

#### 使用预置配置文件
`make <预置配置文件名>`  
例: `make qemu_mips64r6el_malta_defconfig`

#### 调整配置
`make menuconfig`
首先检查`Target options`正确不正确，然后检查`Toolchain`。`Toolchain`里可以选择支持c++，支持openMP，支持wchar等等。

## 编译
编译前可能需要安装一些支持： `sudo apt install kernel-package linux-kernel-generic kernel-common`  
在我的电脑上必须 `export LANG=C` 才能编译通过

## 使用
将交叉编译工具路径加入PATH：  
`export PATH=/home/zww/Downloads/buildroot-2019.02.2/output/host/bin:$PATH`  
即可使用此交叉编译工具链  
例： `./configure --host=mips64el-linux CC=mips64el-linux-gcc  --enable-shared --enable-static --prefix="/home/zww/t/cross/jpeg"`

## 备注
使用configure配置时--host参数若不带操作系统类型（如使用--host=mips64el），则不会编译出动态库。
