+++
date = 2018-03-26
draft = false
slug = "compress-jpg-with-guetzli"
tags = ["Other"]
title = "利用Guetzli压缩jpg图片"
+++

> Guetzli is a JPEG encoder that aims for excellent compression density at high visual quality. Guetzli-generated images are typically 20-30% smaller than images of equivalent quality generated by libjpeg. Guetzli generates only sequential (nonprogressive) JPEGs due to faster decompression speeds they offer.
> Guetzli 是一个JPEG编码器,它可以实现在高质量下出色的压缩比例,Guetzli通常情况下比libjpeg生成的jpeg图片大小要小20%～30%.Guetzli只会生成序列化的JPEGs来保证同时也拥有高速的解码能力.

所以Guetzli事实上是一种图片编码,在拥有高质量高压缩比的同时完全兼容jpg解码.  
#### 编译
###### 安装依赖
On Debian/Ubuntu

```zsh
apt-get install libpng-dev
```
On CentOS/RH

```zsh
yum install libpng-devel
```
On Fedora

```zsh
dnf install libpng-devel
```
On ArchLinux

```zsh
pacman -S libpng
```
On Alpine Linux

```zsh
apk add libpng-dev
```

###### 从源码编译

```zsh
git clone https://github.com/google/guetzli.git
cd guetzli/
make
```
完成后编译好的二进制文件位于`bin/Release/guetzli`.  

#### 使用
编译出来的二进制文件就可以直接使用了.  
Guetzli同时支持对jpg和png图片的编码压缩

```zsh
guetzli [--quality Q] [--verbose] original.png output.jpg
guetzli [--quality Q] [--verbose] original.jpg output.jpg
```
其中`--quality`参数为质量,取值100~85,默认为95.  
压缩一张尺寸为1200x900文件大小为337kb的图片为例,默认压缩后图片大小254k.同时前后2张图片肉眼感官上基本无差别.  

* 压缩前
![](/images/2018/03/general_jpg.webp)

* 压缩后
![](/images/2018/03/jpg_with_guetzli.webp)

#### 和其他图片对比

| 图片类型  | 图片大小   | 兼容性   |备注   |
| --------   | :-----:  | :-----:  | :-----:  |
| jpg        | 小        |   最佳    |         |
| jpg with guetzli | 最小   |最佳    |需要Guetzli做前期编码.编码时间非常长,同时占用大量内存 |
| png         | 大       | 最佳      |        |
| webp         | 比普通jpg更小  | 只有Chrome以及Android原生支持      | 非原生支持的平台需要特定程序做编解码,会消耗额外时间 |

在特定应用环境下,将jpg图片使用Guetzli进行一次性压缩拥有比较大的实用性.


#### 更新
ChromeLabs开源了一个新的工具,整合了几个比较知名的工具.  
其中MozJPEG对jpg的重编码效率更高.  
https://github.com/GoogleChromeLabs/squoosh/

