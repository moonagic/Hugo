+++
date = 2018-07-13
draft = false
slug = "use-acme.sh-to-issue-ca"
tags = ["Linux", "SSL"]
title = "用acme.sh签发Let's Encrypt证书"
+++

这几天用certbot签发Let's Encrypt证书的时候发现了各种问题, 有Python版本问题以及pip源问题.  
反正就是各种蠢  
> 对我这样一个使用者来说Python制造的问题比它解决的问题还多

于是开始使用国人制作的shell工具acme.sh来签发,  
这个工具安装使用很简单,  
安装:
```bash
curl https://get.acme.sh | sh
```
使用:
```bash
# 使用手动dns验证
acme.sh --issue -d example.com --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
# 续签
acme.sh --issue -d example.com --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please --renew
# 续签ecc
acme.sh --issue -d example.com --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please --renew --ecc
# 签发384位密钥的ecc证书
acme.sh --issue -d example.com --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please --keylength ec-384
# 签发泛域名证书
acme.sh --issue -d example.com -d '*.exmaple.com' --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please --keylength ec-384
```
第一次执行的时候会提示你需要设定的`TXT`记录,设定好以后再次执行就可以获取到证书了.
