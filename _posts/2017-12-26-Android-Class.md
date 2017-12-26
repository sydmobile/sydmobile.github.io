---
layout: post
title:  "Android中主要类的继承关系梳理汇总"
date:   2017-12-26 15:14:54
categories: Android
tags: android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}







>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众及时获取更新和我交流互动。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！ 


这一篇文章可能文字相对来说很少，大多是图片，可是花费的时间更多，因为这些图都是我自己一点点设计的，花费的心血也比较多，不为别的，只希望可以对你我有所帮助就足够了。   

首先介绍一下主要内容，下面这张图主要梳理和汇总了我们在Android开发中用到的或者接触到的常见的类，我把这些类的继承关系做了一些梳理，可以让我们看上去更加的醒目和直观。有一个整体的把控。也方便日后查找，如果你是新手的话，可以按照这上面的类，一个一个的使用学习。   

![Android主要类整理](http://img.blog.csdn.net/20171226163840427?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   

你也可以把这张图片保存下载，是不是看一下，看这上面的每一个类，看看你都能想到什么，能对他们说出什么，作为后期复习巩固使用也是不错的。   

下面对这些类有哪些具体的子类做一个梳理。   

首先是 View 这个类（我把一些比较重要或者我们经常使用的类用红笔画出来了）   

![View ](http://img.blog.csdn.net/20171226164455661?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)    

下面对View里面的一些子类做一下展示：  
ImageView  

![ImageView  ](http://img.blog.csdn.net/20171226164714105?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)    

ProgressBar  

![ProgressBar ](http://img.blog.csdn.net/20171226164837840?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

SurfaceView  

![SurfaceView ](http://img.blog.csdn.net/20171226164935540?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

TextView  

![TextView](http://img.blog.csdn.net/20171226165028066?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  


ViewGroup 

![ViewGroup](http://img.blog.csdn.net/20171226165134368?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


下面再对ViewGroup的子类做一个展示：  

AdapterView  

![这里写图片描述](http://img.blog.csdn.net/20171226165324835?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

FrameLayout  

![FrameLayout  ](http://img.blog.csdn.net/20171226165401883?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)     

LinearLayout  

![LinearLayout  ](http://img.blog.csdn.net/20171226165454229?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)    

这些可能看上去都是一些很基础的内容，但是往往你掌握的基础程度决定你能走多远。你对这些列出来的类有多熟悉呢？是不是每一个的属性特性你看到它都能一一说来呢？  

后期我可能会逐个对这里面的类进行汇总总结，希望会对你有一丝帮助。  

![](http://img.blog.csdn.net/20171226170116377?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
欢迎大家关注我的微信公众号，和我交流分享



