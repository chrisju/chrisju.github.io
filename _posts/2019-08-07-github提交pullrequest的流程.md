---
layout:     post
title:      github提交pull request的流程
subtitle:   
date:       2019-08-07
author:     zz
header-img: img/post-bg-younai.jpg
catalog: true
tags:
    - 编程
    - git
    - github
---

> github上给他人项目做贡献，提交pull request的流程

1. 在他人项目页面fork他人项目
2. 用`git clone`下载项目到本地
3. 将他人项目设为upstream远程库 `git remote add upstream https://github.com/guoyunsky/Markdown-Chinese-Demo`
4. 拉取upstream `git fetch upstream`
4. 建立提交分支 `git co -b spin upstream/master`
5. 修改或应用自己的patch, 产生本地提交
6. 将分支提交到origin上 `git push origin spin:spin`
7. 在github上自己的此分支页面上点击"New pull request"

