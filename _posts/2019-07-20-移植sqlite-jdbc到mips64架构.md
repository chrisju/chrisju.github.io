---
layout:     post
title:      移植sqlite-jdbc到mips64架构
subtitle:   
date:       2019-07-20
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
    - 交叉编译
    - 编程
---

> 在linuxmint19上使用龙芯编译器交叉编译mips64el的libsqlitejdbc.so
> sqlite-jdbc的版本为3.7.2

## 准备
* 下载sqlite-jdbc <https://github.com/xerial/sqlite-jdbc/releases> 版本3.7.2

## 编译
1. 需要有交叉编译环境
2. 修改Makefile.common，添加mips64支持，并将target强行设定为Linux-mips64el
3. `mvn clean && mvn compile`，最后失败没事，只要so编译出来就行  
```
--- a/Makefile.common
+++ b/Makefile.common
@@ -40,12 +40,14 @@ endif
 
 # os=Default is meant to be generic unix/linux
 
-known_targets := Linux-i386 Linux-amd64 Mac-i386 Mac-x86_64 Windows-x86 Windows-amd64
+known_targets := Linux-i386 Linux-amd64 Mac-i386 Mac-x86_64 Windows-x86 Windows-amd64 Linux-mips64el
 target := $(OS_NAME)-$(OS_ARCH)
 
 ifeq (,$(findstring $(strip $(target)),$(known_targets)))
   target := Default
 endif
+target := Linux-mips64el
+$(warning target:  $(target))
 
 Default_CC        := gcc
 Default_STRIP     := strip
@@ -68,6 +70,13 @@ Linux-amd64_LINKFLAGS := -shared
 Linux-amd64_LIBNAME   := libsqlitejdbc.so
 Linux-amd64_SQLITE_FLAGS  := 
 
+Linux-mips64el_CC  := mips-linux-gnu-gcc
+Linux-mips64el_STRIP  := mips-linux-gnu-strip
+Linux-mips64el_CFLAGS    := -I$(JAVA_HOME)/include -Ilib/inc_mac -Os -fPIC -mips64r2 -mabi=64
+Linux-mips64el_LINKFLAGS := -shared
+Linux-mips64el_LIBNAME   := libsqlitejdbc.so
+Linux-mips64el_SQLITE_FLAGS  := 
+
 Mac-i386_CC        := gcc -arch $(OS_ARCH) 
 Mac-i386_STRIP     := strip -x
 Mac-i386_CFLAGS    := -I$(JAVA_HOME)/include -Os -fPIC 
```

## 使用
查看sqlite-jdbc源码中的SQLiteJDBCLoader.java的loadSQLiteNativeLibrary函数可知，可通过设置"org.sqlite.lib.path"参数来调用下面的"libsqlitejdbc.so"。  
示例： `java -Dorg.sqlite.lib.path=/usr/lib64 testjdbc.jar`  
