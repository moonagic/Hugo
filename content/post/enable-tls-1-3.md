+++
date = 2017-09-17
draft = false
slug = "enable-tls-1-3"
tags = ["Nginx", "SSL"]
title = "为Nginx添加TLS1.3支持"
+++

Nginx主线分支从1.13.0版本开始支持TLS1.3,只需要在编译的时候选择使用OpenSSL支持TLS1.3的分支进行编译即可.

#### 使用对应的OpenSSL分支进行编译
```bash
# OpenSSL对TLS1.3的支持已经到了draft19,不过Chrome和Firefox对TLS1.3的支持还在draft18
git clone -b tls1.3-draft-18 --single-branch https://github.com/openssl/openssl.git openssl
```
然后在预编译的时候选择该分支,并添加额外选项`--with-openssl-opt=enable-tls1_3`.
其他操作参考[手动编译Nginx支持ALPN,以在最新版Chrome中支持HTTP/2](https://moonagic.com/support-http2-on-chrome-with-compile-nginx/).

#### 浏览器设定

Firefox目前最新版已经默认开启TLS1.3支持(**如果不是新安装的Firefox那么可能依然需要检查`about:config`中的对应设定**).
Chrome需要在`chrome:flags`中将`TLS 1.3`选项调整为`Enabled (Draft)`

#### 测试

Firefox:
![](/images/2017/09/Screenshot-2017-09-17-213944.webp)

Chrome:
![](/images/2017/09/Screenshot-2017-09-17-212140.webp)


#### 已知问题

由于`nginx-ct`目前并不支持TLS1.3,所以如果`Certificate Transparency`是靠该方案实现的话那么开启TLS1.3后无法继续显示`Certificate Transparency`信息.  
> 该问题在Letsencrypt自带CT信息后得到一定缓解,某些商业证书也不需要`nginx-ct`来实现CT信息.

#### 更新(September,12,2018)
TLS1.3最终定稿[rfc8446](https://tools.ietf.org/html/rfc8446),Openssl-1.1.1在几个小时前也正式发布支持.  
目前Firefox Nightly版已经支持,Chrome从70开始也支持了.