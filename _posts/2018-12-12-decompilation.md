---
layout: post
title:  "Android 反编译"
date:   2018-12-12 15:14:54
categories: Android
tags: android 反编译
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}










大家一说到反编译可能脑海中会首先想到不好的一面，破解别人的 APK 之类的。其实大可不必这么想。商业级别的 APK 也没有那么容易被你钻漏洞。一些核心的业务处理会在后台进行操作。本地的 APK 也会进行混淆加密等。所以我们进行反编译主要还是进行学习，看看别人怎么实现的，自己有个思路而已。

>文章最早发布于我的微信公众号  **Android开发者家园**       
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

关于这方面的资料，网上也有很多，这里就简单说一下过程，达到能够使用的目的！

这里介绍两种方式：

# 1.在线反编译

这种方式很简单，我们只需要打开网址，把我们的 AKP 放进去就可以进行在线反编译了。不过过程会有点慢。网址：http://www.javadecompilers.com/apk  

![反编译网址.jpg](https://upload-images.jianshu.io/upload_images/6737388-30d6c1ecb732ee2c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

操作很简单，就不再一步一步的进行演示了。

# 2.利用编译工具反编译

利用工具，自己进行反编译（其实就是工具的使用）

需要的工具：

- APK TOOL ：谷歌提供的 APK 编译工具，可以反编译和回编译。我们都知道，其实 APK 就是一个压缩包，我们完全可以把 .apk 修改为 .zip。通过这种方式来获取资源文件，但是 xml 会乱码。如果你使用 apk tool 来进行反编译 apk 就不会出现这种问题了。 下载地址：https://ibotpeaches.github.io/Apktool/install/ （需要科学网）
- dex2jar：将 dex 文件转换成 jar 包 下载地址：http://sourceforge.net/projects/dex2jar/files/
- jd-gui：用来查看 jar 包里面的代码的一种工具。官网下载地址：http://jd.benow.ca/ 

如果你不方便科学上网的话，我已经打包了，可以在这里下载：https://download.csdn.net/download/sydmobile/10846283

## APK TOOL 的使用

使用很简单，把要反编译的 apk 放到 apktool.jar 所在的目录，然后在命令行中定位到当前文件夹。然后输入命令： `apktool d xxx.apk` 这样就成功了。会在当前目录下生成一个以 apk 命名的目录，这个目录就是解压出来的目录。

## dex2jar 使用

把 apk 解压后的 classes.dex 文件放到 dex2jar 所在的文件夹中。然后在当前目录下在命令行中输入： `d2j-dex2jar classes.dex`  就会生成一个 classes.jar 包。然后用 jd-gui 打开就可以了。

以上内容很简单，就不再演示了。有什么问题可以交流


![欢迎大家关注我的微信公众号，和我交流分享](http://upload-images.jianshu.io/upload_images/6737388-1eca35c3d7e04a1e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 


