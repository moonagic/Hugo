+++
date = 2018-08-29
draft = false
slug = "dualfisheye-to-equirectangular"
tags = ["Other"]
title = "利用双鱼眼图制作全景图"
+++

许多全景相机,如[米家全景相机](https://www.mi.com/mj-panorama-camera/),由2个鱼眼相机组成.  
生成的文件为鱼眼镜头图片,如下图
![](/images/2018/08/360_0077.webp)

如果需要将双鱼眼图片制作成为全景图很多时候需要安装专业软件,比如[PTGui](https://www.ptgui.com).  
偶然在Github上发现了一个相关项目[dualfisheye2equirectangular](https://github.com/raboof/dualfisheye2equirectangular)是特地满足这样的需求.  
按照README操作后可以成功生成如下全景图原图
![](/images/2018/08/out.webp)

生成的原图在两张鱼眼接缝的对方会有重叠,因为使用了大于180度的鱼眼镜头.
