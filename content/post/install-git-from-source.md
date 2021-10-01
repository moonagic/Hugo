+++
date = 2017-05-05
draft = false
slug = "install-git-from-source"
tags = ["Git"]
title = "从源码编译安装Git"
+++

##### 编译依赖
```bash
apt-get install libcurl4-gnutls-dev libexpat1-dev gettext zlib1g-dev libssl-dev
```
##### 下载
到[Github](https://github.com/git/git/releases)下载需要的版本
##### 安装
```bash
autoconf
./configure prefix=/usr/local all
make
make install
```
