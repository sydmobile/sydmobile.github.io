---
layout: post
title:  "关于Android源码的理解以及如何阅读源码"
date:   2017-11-07 08:01:02
categories: Android
tags: Android源码 阅读源码
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}









>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

平时我们在用Android Studio开发项目的时候，如果想要查看源码，直接 Ctrl + 左键 查看对应的源码，你可能会发现有一些具体源码看不到，或者部分代码飘红。我们就来说说这一部分。  
### IDE查看源码出现的问题
我们的Android项目都是需要依赖Android SDK里对应的API Level的android.jar包的，这样才可以使用Android提供的API，在IntelliJ里面，查看具体类的源码的时候，如果Android SDK里对应的API Level的Source包有下载的话，IDE会打开对应的Source包，如果没有下载的话，IDE会把对应的API Level的android.jar包反编译场成Java代码，这个规则对于其他的一些第三方的开源项目也是一样的。但是你最好还是下载Source源码来查看，有的时候反编译的Java代码不可能完全和源代码一样，有时候反编译的代码的执行逻辑可能完全等价，但是可阅读性不好，有可能会缺少重要的代码注释。


因为Android SDK自带的Source源码包很小（你可能会说已经好几个G了还小啊，后面会讲到真正的Android源码，哪个时候你就知道了），并没有包括所有的Android Framework的源代码仅仅是提供给应用开发者参考使用，有一些比较少的系统类的源码没有给出，所有你有可能在查看源代码的时候看到这种情况   
RuntimeException（“Stub”）
![runtimeexception](http://img.blog.csdn.net/20171107152008877?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
查看代码的时候遇到这种情况，表示实际运行的时候逻辑会到Android ROM（这里Android ROM可以理解为你Android手机的Android系统，里面同样也包含了你在开发的时候用到的类）里面找相对应的类和方法来代替执行。   
此外我们在IDE中查看源代码的时候，还会经常看到一些类和方法中会出现报红（也就是找不到）的情况，这种情况在我们查看源代码的时候是很常见的。
![飘红](http://img.blog.csdn.net/20171107154853775?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
    
这种情况并不是说这些方法或者变量是不存在的，而是这些方法因为出于安全或者某些原因被我们使用的Android ＳＤＫ隐藏了，这些API不直接暴露给应用层的开发者，这些类和方法在Android源码编译完成的android.jar包里面会把这些API隐藏（如果是你自己编译的Android源码的话就不会了，后面的文章会讲），而我们的Android项目是依赖这个编译后的android.jar包的，所以我们在查看源码的时候，IDE就会自动去android.jar里面找对应的API，所以就会出现这种情况了，实际上这种ＡＰＩ同样在ＲＯＭ中是存在的，有些开发者发现了一些可以修改系统行为的隐藏ＡＰＩ，在应用层通过反射的方式强行调用这些API执行系统功能，这种手段也是一种HACK。   

上面讲了我们在IDE中直接查看源码有可能会出现的问题，下面就介绍一下怎么查看完整的源码。
### 查看完整源码
当你需要查看完整源码的时候，需要去[AOSP（Android Open Source Project）](https://android.googlesource.com/)项目里面找了，（需要科学上网）这个里面放着Android真正的完整源码，这里所说的完整源码不仅仅包括Android系统的源码还包括了一些开发工具比如：aapt、adb等等。
![android源码](http://img.blog.csdn.net/20171107155005066?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
选择自己需要的下先来，如果全部下载下来估计有几个T吧，不过其实是不需要的，作为应用层的开发，我们看[应用层源码](https://android.googlesource.com/platform/frameworks/base/+/master/core/java/android/)就好了
![应用层源码](http://img.blog.csdn.net/20171107155029606?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

介绍查看源码工具：

1. Chrome扩展工具
Android SDK Search
使用这个插件在谷歌浏览器中，打开Android的官网查看API说明的时候，会有个按钮，通过点击这个按钮就可以直接跳转到AOSP中对应的源码中
![](http://img.blog.csdn.net/20171107155455111?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![](http://img.blog.csdn.net/20171107155518512?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
注意仅适用于Android开发者官网上查看API文档，不适用国内的那个镜像网址。
2. Source Insight查看完整源码     
一个强大的查看源码的软件，把你在AOSP中下载的源码导入到这个软件中就可以快速的查看源码了   
![Source Insight](http://img.blog.csdn.net/20171107155957469?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 破解方法：  
 http://www.cnblogs.com/Napoleon-Wang/p/6706773.html       
  具体操作方式：   
  http://blog.csdn.net/shulianghan/article/details/50553001
3. 直接将编译后源码导入Android Studio查看    
具体操作步骤：   
http://www.jianshu.com/p/fb16fa459acf     
关于源码的学习：         
http://www.jianshu.com/p/a4b40a9d1b4f   


> 参考：     
http://kaedea.com/2016/02/09/android-about-source-code-how-to-read/

<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
关注微信公众号，及时获取内容更新
</p>
