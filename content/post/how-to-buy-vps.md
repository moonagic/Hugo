+++
date = 2018-03-31
draft = false
slug = "how-to-buy-vps"
tags = ["VPS"]
title = "如何选购VPS"
+++

---
### 什么是VPS
VPS全称Virtual Private Server(虚拟专用服务器).其实就是物理机上开出的虚拟机.  
很多IDC服务商都喜欢给自己的VPS起一个听起来一脸懵逼的名字比如阿里云的ECS,比如腾讯云的CVM等等.  
直接说**主机**或者直接说**实例**感觉在他们的企业文化里很难接受似的,一定要起个名字.  
其实我想原因是AWS这个云服务的鼻祖把他们的机器取名EC2.  
> 有些人不管什么时候都说ECS,就是典型的只用过阿里云的傻缺.  

<!--more-->
---
### VPS虚拟化的分类
目前市场上的虚拟化技术主要分为4类:
###### 1.XEN
XEN的典型代表是早期的AWS和老牌VPS服务商Linode,不过这两家都迁移到了KVM.目前主流服务商已经全面放弃XEN.
###### 2.KVM
目前的主流全虚拟化技术,全面替代了XEN.XEN算是Linux的一个应用,而KVM是Linux的一个模块.  
下面提到的服务商除了微软以外都是使用的KVM.
###### 3.Hyper-V
微软自家虚拟技术,Windows专享.(不然你以为Azure的母鸡是一堆Linux?)
> 本来Hyper-V在这个领域没什么存在感,但耐不住Azure这玩意市场占有率高啊

###### 4.OpenVZ
OpenVZ本质上并不是虚拟化,而是容器.相对于XEN和KVM而言它的性能损失是最小的(几乎可以忽略不计),并且内存/CPU/硬盘伸缩不用重启(XEN和KVM是需要重启后才能正确配置的).  
而OpenVZ最大的缺点在于虚拟隔离化非常低,并且硬件层面权限较低甚至不能拥有自己的独立Linux内核.  
同时OpenVZ最大的问题并不在于技术层面,而在于绝大部分服务商使用OpenVZ的目的是内存可以超售.超售是什么概念呢?简单说一台32G内存的物理机可以开出64台内存1G的VPS.
###### 其他
除了上面说到的以外还有一些非主流的虚拟技术,比如VMWare这种商业解决方案(当然价格嘛...).不过各种其他虚拟技术加起来份额不到1%.

---
### VPS的网络
网络也分几个考虑方向,延迟 带宽 丢包率等等:
###### 1.延迟
延迟在路由正常的情况下基本等同于物理距离.  
在条件允许的情况下选择就近的城市是最理想的,比如重庆的话第一考虑阿里云/腾讯云的广东/深圳,或者直接选择腾讯云的成都.  
不过如果要选择大陆以外的机房的话就比较复杂了,需要考虑不同的城市不同的ISP等等.比如南方电信/联通第一选择是走香港,北方第一选择去韩日等等.  
如果要选北美的话,美国西部的洛杉矶/西雅图/旧金山等地区是第一选择,从广东或者上海到美西延迟理想的话可以做到150以内.

###### 2.带宽
各个地区的网络发展水平直接决定了带宽水平,在欧美机房普遍G口(欧洲甚至有10G口的机房)的情况下国内还在按M带宽在租售.(或者选择按流量计费可以将带宽开到100M或者200M,不过又需要面对天价流量费)  
而绝大部分的国外IDC服务商都会在购买VPS的时候给大量的流量配额同时有足额的带宽(一般在数百Mbps,100M的情况是很少的),例如Linode DigitalOcean Vultr这三家最廉价的套餐都有1T的流量:
![](/images/2018/03/linode_price.webp)

同时,云服务三巨头(AWS, Azure, Google Cloud)的做法则是放开带宽(很多时候突发带宽可以跑满G口),而收取流量费用.
国内的话除了一些非主流IDC以外几乎都是上行5M-10M带宽的机器.

###### 3.开挂的线路
国内特殊的网络环境,AWS Cloudflare Akamai Google等等因为你懂的原因无法在国内建立CDN节点,直接造成每个人对这些节点的访问都需要走国际出口,导致大陆的国际出口几乎一直是处于拥堵状态.  
不过**加钱世界可及**嘛...中国电信有第二代骨干网也就是CN2,中国联通也有Global VIP线路.接入这些线路的机房肯定比接普通线路的机房访问速度和稳定性高很多,就跟开了挂一样.  
另外还有一些冷门线路,虽然路由并不好但是好在使用的人少所以延迟和稳定性也有保证.

---
### VPS的性能
VPS的配置主要就是CPU 内存 硬盘大小和读写性能等等.  
不同的服务商提供的硬件性能差异很可能是非常大的,有可能一台4核的机器编译同一套代码还不如别人单核的机器编译的快.  
硬盘的话读写性能也非常重要,特别是要跑数据库这种.性能好的硬盘读写我遇到过1000M/s的,而遇到过最差劲的为5M/s.  
内存差异并不像CPU和硬盘,普遍性能差异不会太大,**不过非ECC内存的服务器千万别买**.  
如果购买服务器的目的是做站的话性能是必须要考虑的东西,找个闲时跑一次**unixbench**是很好的判定性能的办法.

---
### VPS的品牌
品牌这个东西嘛还是挺重要的,某些老牌服务商完全可以放心购买但是新服务商的话就说不定了,机器或者网络不稳定甚至跑路都时有发生.  
###### 1.老牌服务商
以Linode,DigitalOcean,Vultr,Ramnode,Bandwagonhost为代表.  
优点:  
* 机器稳定行都有足够保证
* 价格实惠,并且随着时间会不断降价
* 都是纯SSD硬盘不会像国内的服务商那样默认给你石头盘,想要SSD还得加钱
* 有一定的抗攻击能力

缺点:
* 太出名,使用的人太多,特别是国人.

###### 2.国内云服务商
以阿里云腾讯云为代表  
优点:
* 国内机房延迟非常理想

缺点:
* 国内域名需要备案
* 机器升降配限制多
* 各种套路,这次阿里云优惠价格买的机器是CPU负载只能用到10%那款你他妈敢信?
* 无法按小时计费或者如果小时计费最终费用会比包月计费高出几倍.
* 要么接受小水管带宽,要么按流量支付费用.
* 国内公司技术受限,或者*为了利润刻意限制某些功能*.比如Azure免费的内网高速通道,阿里云一个月费用大概需要几万.比如别人升降配只需要点一个按钮阿里云还要你续一个月的费等等.甚至阿里云出现故障排障后必须更换IP你敢信?

###### 3.国际云服务商
以Azure,Google Cloud为代表.  
AWS由于特定原因,被重点照顾后网络质量很一般.  
至于IBM嘛,不管是他的Softlayer(有一段时间电信走CN2,不过架不住中国人多而且有锐速这种流氓只能切换了线路)还是容器云路由对大陆都相当不友好,不在讨论范围内.  
优点:
* 特定机房对大陆网络非常优秀
* 按小时计费
* 足够大的带宽
* 真云
* 计费规则复杂,但没有套路

缺点:
* 国内购买可能会遇到问题

> 顺便一提,从市场占有率统计上看这4家云服务公司后面就是阿里云了,现在大陆互联网全民创业买服务器的太多了,硬撑出来一个老四.

###### 4.国人小公司
国人小公司就比较复杂了,有些确实是公司,而有些则是一个人或者几个人搞起来的(俗称oneman)
> oneman并不是指你只有一个人或者你是一个工作室.
或许你有几个人甚至成立了公司,但是你的技术支持你的体量根本无法像一家公司一样运作,请几个兼职当客服甚至请不起客服一个工单过了几天都无法回应你哪来的勇气说你不是oneman?  
Hostigation很长时间就只有老板自己一个人,Ramnode事实上是夫妻档,但是有人敢说别人是oneman?!  

oneman这种嘛,不遇到问题则罢,一旦遇到问题轻则服务停个几天重则服务商直接跑路都是可能的.(比如前两天的吾云)  
**甚至最开始就是打算割韭菜的也不在少数**  
所以国人小公司并不建议购买年付.

不过国人小公司的优点嘛也是显而易见的,就是机器位置和线路针对国人需求优化满足特定需求.  
另外这一类的服务商还很容易被攻击,而且几乎可以肯定就是在相互攻击抢客户,被这样打垮的小公司不要太多.

---
### 我的选择
###### **Google Cloud**
使用第4年了,除了CPU和流量太贵以外没什么缺点.网络质量非常优秀,不过自从一年多以前Google自作聪明将300美金的试用额度从3个月期限延长到12个月以后每况愈下,只能说**Google不懂中国人**.
###### **Azure**
微软的东西花得起钱的话还是非常好的,如果花不起钱的话那么..我告诉你吧,前面写文件5m/s的例子就是Azure的最廉价款.
###### **阿里云国际**
网络质量还好,其他就算了.