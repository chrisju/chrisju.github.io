---
layout:     post
title:      交叉编译tesseract
subtitle:   
date:       2019-06-04
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
    - 交叉编译
---

> 在linuxmint19上交叉编译mips64的libtesseract.so及liblept.so
> 类似 <https://www.wandouip.com/t5i227081/> ，不过在我电脑上完善了一下。tesseract的版本为3.05.02

## 准备
* 下载zlib <http://www.zlib.net/> 版本1.2.11
* 下载jpeg <http://www.ijg.org/> 版本v9
* 下载libpng <https://github.com/glennrp/libpng/archive/v1.2.58.tar.gz> 版本1.2.58
* 下载leptonica <http://www.leptonica.org/> 版本1.76.0
* 下载tesseract <https://github.com/tesseract-ocr/tesseract/archive/3.05.02.tar.gz> 版本3.05.02
* 创建一个文件夹存放编译结果: `mkdir /home/zww/t/cross`

## 编译zlib
```
export CC=mips64el-linux-gcc
./configure --shared  --prefix="/home/zww/t/cross/zlib
make && make install
```

