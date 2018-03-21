---
layout: post
title:  "Button一个有趣的阴影问题"
date:   2018-03-21 15:14:54
categories: Android
tags: android
excerpt: 发现这个问题还是在工作中，我们的UI设计师发现我们的APP上，在登录页面的登录按钮，下面有阴影效果，但是注册页面却没有阴影效果。
mathjax: true
author: sydMobile
---
* content
{:toc}












发现这个问题还是在工作中，我们的UI设计师发现我们的APP上，在登录页面的登录按钮，下面有阴影效果，但是注册页面却没有阴影效果。    
  
![图片](http://img.blog.csdn.net/20180321115012999?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

![图片](http://img.blog.csdn.net/20180321115217813?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


效果就像是上面的那样，可能在图片上不是很明显，我们可以发现在登录按钮下，有一个小阴影，而注册按钮是没有的。最初还以为是Button的背景有区别呢，查看后发现，背景用的都是一样的，都是一个shape文件。那就只能在布局中查看有什么不同了，查看布局得到，在登录页面Button的父布局LinearLayout的高度是match_parent，而在注册页面Button父布局的高度是wrap_content，这是他们的唯一不同之处。果然如果把wrap_content修改成match_parent的话，注册的button下面也会有阴影了。  

你以为这就完事了吗？哈哈，并没有！
我又通过代码进行了实验，实验了什么内容呢？直接上图！    

![button](http://img.blog.csdn.net/20180321164653695?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)        

看看上面的图片中的button有什么不同吗？仔细观察的话，会发现从1号到9号button下面的阴影会逐渐加强，最上面的1号看上去就直接没有阴影。而它们的代码都是相同的，唯一不同的就是在付父布局中的位置。由此可以发现当button在屏幕的下方的时候，并且父布局不是wrap_content的话，会出现阴影效果（如果父布局大小写死，并且比button要的时候也会出现阴影），而在屏幕上方的button就没有这个问题。   

![TextView](http://img.blog.csdn.net/20180321170554821?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

这是使用的 TextView 就没有阴影效果。   

好了，今天就分享这么一个小问题，以后大家遇到这种情况就知道是怎么回事了。     



>文章最早发布于我的微信公众号  Android开发者家园 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

![欢迎大家关注我的微信公众号，和我交流分享](http://img.blog.csdn.net/20180301210553345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

