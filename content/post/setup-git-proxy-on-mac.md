+++
date = 2016-08-04
draft = false
slug = "setup-git-proxy-on-mac"
tags = ["Proxy", "Mac", "Git"]
title = "在Mac上为Git配置代理"
+++

Mac上为iTerm2设置代理可以直接用
```bash
export http_proxy=http://127.0.0.1:6152/
export https_proxy=http://127.0.0.1:6152/
```
但是对git并不那么奏效,因为git除了走https外还需要走ssh.
试了网上很多办法都不行,最后找到了这个办法:  
~~先将[connect](https://bitbucket.org/gotoh/connect)(我自己clone到了[github](https://github.com/moonagic/connect))克隆到本地~~  
==bitbucket上的版本需要做出一项改动才能在Mac上正确编译使用,还是直接使用GitHub上已经修改好的仓库吧==[^diff]  
```bash
git clone https://github.com/moonagic/connect.git
cd connect
# 直接gcc编译
make
# 然后将编译得到的文件connect拷贝到bin目录
cp connect /usr/local/bin
```
修改ssh配置文件(没有的话新建一个)
```bash
vi ~/.ssh/config
```

```bash
# 全局
ProxyCommand connect -S 127.0.0.1:1080 %h %p
# 只为特定域名设定
Host github.com
        ProxyCommand connect -S 127.0.0.1:1080 %h %p
```
其中-S代表走socks代理,如果需要使用HTTP/HTTPS代理的话使用-H

现在走ssh协议的git就可以走代理了.

[^diff]: 改动为将`signal(SIGALRM, (__sighandler_t)sig_timeout);`注释掉.
