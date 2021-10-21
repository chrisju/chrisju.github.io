---
layout:     post
title:      宏基aspire7安装linux
subtitle:   
date:       2021-10-20
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - linux
---

宏基aspire7无法直接安装linux，会遇到Intel的固态硬盘驱动不兼容问题，宏基BIOS无SATA MODE设置的问题，Secure Boot的问题。

**注意，步骤不对会导致windows无法启动或数据丢失**

1. win10中卸载intelRST驱动（控制面板中的程序管理里可以卸载）。
1. 按win+R，在运行窗口中输入msconfig，选择引导->引导选项->安全引导，打上勾并保存设置.
1. 重启电脑，按F2进入BIOS设置，然后按CTRL+S打开隐藏设置，之后再调整BIOS的SATA硬盘控制器的模式由RAID改为AHCI.（RST使用的是RAID模式，因为卸载了RST驱动，所以需要改变硬盘控制器模式），并将SECURE BOOT设为disable，保存设置。
1. 重新启动后会自动进入安全模式（因为改变了设置，无法正常进入w10，所以之前的安全引导可以让电脑自动进入安全模式并且修复硬盘控制器模式改变而出现的问题）。
1. 再次按win+R，输入msconfig进入系统配置，这次把安全引导选项去掉。再重启电脑，进入ubuntu安装，就会发现可以正常安装了同时w10也可以正常进入。

参考了 `https://blog.csdn.net/mikow/category_10116273.html`
