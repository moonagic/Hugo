+++
date = 2016-11-09
draft = false
slug = "try-bbr"
tags = ["Linux"]
title = "Google的新TCP拥塞算法BBR"
+++

> 更新:**Debian9都发布好久了,用Debian9吧 不需要折腾内核就能直接开启BBR**

上个月网友发现Google在GitHub上的项目[Google/BBR](https://github.com/google/bbr).
前几天发现在几个Linux发行版中的候选版内核已经实装,而里面刚好也有Debian.

在看了Telegram群组里的说明后自己试着新开一台机器用上了BBR.

对比测试后发现提升确实非常大,gce美西在试用默认算法的情况下重庆电信HTTP下载大概在100~200K/s左右,而切换到BBR以后HTTP下载速率可以达到3000~4000K/s.

**Debian系统具体步骤:**

* 添加experimental源

```bash
deb http://httpredir.debian.org/debian experimental main
```
* 安装新内核

==~~目前最新4.9内核预选版为rc8~~==

```bash
apt -t experimental install linux-image-4.9.0-rc8-amd64-unsigned
```
==~~目前的版本~~==
```bash
apt -t experimental install linux-image-4.9.0-trunk-amd64-unsigned
```
==**进入unstable源**==
```bash
deb http://httpredir.debian.org/debian unstable main
```
添加源后可通过`apt-cache search [packagename]`搜索对应包,比如你想要搜索内核的话:
```bash
#apt-cache search linux-image
linux-headers-3.16.0-4-amd64 - Header files for Linux 3.16.0-4-amd64
linux-image-3.16.0-4-amd64 - Linux 3.16 for 64-bit PCs
linux-image-3.16.0-4-amd64-dbg - Debugging symbols for Linux 3.16.0-4-amd64
linux-image-amd64 - Linux for 64-bit PCs (meta-package)
linux-image-amd64-dbg - Debugging symbols for Linux amd64 configuration (meta-package)
linux-headers-4.8.0-0.bpo.2-amd64 - Header files for Linux 4.8.0-0.bpo.2-amd64
linux-headers-4.8.0-0.bpo.2-rt-amd64 - Header files for Linux 4.8.0-0.bpo.2-rt-amd64
linux-image-4.8.0-0.bpo.2-amd64-dbg - Debugging symbols for Linux 4.8.0-0.bpo.2-amd64
linux-image-4.8.0-0.bpo.2-amd64-unsigned - Linux 4.8 for 64-bit PCs
linux-image-4.8.0-0.bpo.2-rt-amd64-dbg - Debugging symbols for Linux 4.8.0-0.bpo.2-rt-amd64
linux-image-4.8.0-0.bpo.2-rt-amd64-unsigned - Linux 4.8 for 64-bit PCs, PREEMPT_RT
linux-headers-4.8.0-2-grsec-amd64 - Header files for Linux 4.8.0-2-grsec-amd64
linux-image-4.8.0-2-grsec-amd64 - Linux 4.8 for 64-bit PCs, Grsecurity protection
linux-image-grsec-amd64 - Linux image meta-package, grsec featureset
linux-image-rt-amd64 - Linux for 64-bit PCs (meta-package), PREEMPT_RT
linux-image-rt-amd64-dbg - Debugging symbols for Linux rt-amd64 configuration (meta-package)
linux-image-4.8.0-0.bpo.2-amd64 - Linux 4.8 for 64-bit PCs (signed)
linux-image-4.8.0-0.bpo.2-rt-amd64 - Linux 4.8 for 64-bit PCs, PREEMPT_RT (signed)
linux-image-4.9.0-trunk-amd64-unsigned - Linux 4.9 for 64-bit PCs
linux-image-4.9.0-rc8-amd64-unsigned - Linux 4.9-rc8 for 64-bit PCs
linux-image-4.9.0-1-amd64 - Linux 4.9 for 64-bit PCs (signed)
```

**安装时可能会提示** *`Depends: linux-base (>= 4.3~) but 3.5 is to be installed`*, 解决办法:
```bash
# 如果没有添加backports源需要手动添加
apt-get install linux-base -t jessie-backports
```
安装结束后需要重启.
重启后`uname -a`查看到的信息应该会显示内核已经切换到4.9.0rc了
```bash
Linux magic 4.9.0-rc8-amd64 #1 SMP Debian 4.9~rc8-1~exp1 (2016-12-05) x86_64 GNU/Linux
```
这时候表示已经安装成功.

* 更新sysctl配置
编辑`/etc/sysctl.conf`

```bash
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```
编辑完成后通过`sysctl -p`更改.
