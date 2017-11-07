---
layout: post
title:  "complileSDKVersion、buildToolsVersion、minSDKVersion、targetsdkversion"
date:   2017-08-29 09:14:54
categories: Android
tags: complileSDKVersion buildToolsVersion  minSDKVersion  targetsdkversion
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}










>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

**complileSDKVersion** 就是表示：编译版本，所谓的编译版本就是我们在开发apk的时候，我需要的类，比如Activity等等要依赖的编译包。我们调用的API都有对应的版本号。    
 
 **buildToolsVersion**  表示构建工具的版本，里面包含很多的打包工具比如：aapt、aidl、dexdump等等
你可以使用一个高版本的构建工具（build-tool）去构建一个使用complile低版本的工程，也就是说 compilesdk的版本可以比buildtools版本低。

**minSdkVersion**：就是你的apk支持的最低版本，也就是应用程序兼容的最低版本，也就是说你的apk要在这个版本以上运行才可以

 **targetsdkVersion** ：声明您已经在这版本上优化了应用程序的最高版本，通俗来说就是，这个版本是你的应用程序适配的最高的一个版本。但是在Android中高系统是向下兼容的，所以就算你的targetsdkversion是20，如果你在版本号是23的手机上运行你的应用程序也是可以的，那么有人会说了那这个还有什么用啊，作用就是 如果你的targetsdkversion的版本如果是20的话，手机是会默认以兼容的方式在运行的。如果21的系统上有些API做出了改变，那么是不会提醒的。比如，在Android4.4中有行为变更，那就是使用AlarmManager API创建的警报默认情况下是不准确的，因此系统会批量应用程序警报并保留系统电源，但是如果你的targetsdkversion低于4.4的话就没有这种行为改变了。
 <br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
关注微信公众号，及时获取内容更新
</p>
