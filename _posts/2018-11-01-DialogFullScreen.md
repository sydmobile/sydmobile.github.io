---
layout: post
title:  "Dialog 宽度占据全屏"
date:   2017-08-29 15:14:54
categories: Android
tags: Dialog全屏
excerpt: 关于如何自定义设置 Dialog 的大小，以及如何让宽度占满整个屏幕，其实是一个老生常谈的内容了，特别是对于很多新手来说。关于这方面的内容网上一搜一大把
mathjax: true
author: sydMobile
---
* content
{:toc}








# Dialog 宽度占据全屏

关于如何自定义设置 Dialog 的大小，以及如何让宽度占满整个屏幕，其实是一个老生常谈的内容了，特别是对于很多新手来说。关于这方面的内容网上一搜一大把。我也看了一下，大多数是互相抄袭。来来回回就是那么几句代码。真实的运行结果往往并不是占满屏幕。这篇文章是把很多常见的情况都举例了。我们先看 Dialog 占满屏的效果，好了下面一步一步看，如果不想看过程可以直接跳过看总结。


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-bab9f4c0626814bc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




## 正常显示全屏

一般的设置宽度占据全屏的效果

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
// window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
window.setAttributes(layoutParams);       //window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
//window.getDecorView().setBackgroundColor(Color.GREEN);
```

Dialog 设置成了点击外部，Dialog 消失。当你点击 Dialog 周围时的时候，Dialog 不消失，说明 Dialog 窗口还包含了周围的一点空间。


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-f944dcd6c39c1171.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![效果图]


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-1e3d65246e01e6b4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图：可以看到 Dialog 的最底层 View 其实是 FrameLayout （也就是 DecorView。默认是有 padding 的，大小为 42，可以通过 mWindow.getDecorView().getPaddingTop()  来获取。如果给 DecorView设置一个背景颜色那么就更加明显了）

## 正常显示全屏-DecorView 设置背景色

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
// window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
window.setAttributes(layoutParams);       //window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
window.getDecorView().setBackgroundColor(Color.GREEN);
```


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-e1aea7079615ec3c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


设置了背景色后就很明显了，周围的一圈绿色就是最底层 DecorView 的 padding 。所以 Dialog 设置成了点击外部，Dialog 消失。当你点击 Dialog 周围时的时候，Dialog 不消失。

## 正常显示全屏-DecorView 设置背景色和最小宽度

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
//window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
window.getDecorView().setBackgroundColor(Color.GREEN);
```


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-2c0cea1e89eb32ef.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




## 正常显示全屏-DecorView 设置背景色和最小宽度和 padding 为 0

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
window.getDecorView().setBackgroundColor(Color.GREEN);
```


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-851534c9ec8582b9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个时候是真正的全屏了。

## 正常显示全屏-DecorView 设置背景颜色和 padding 为 0

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
//window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
window.getDecorView().setBackgroundColor(Color.GREEN);
```


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-c354088fb8f34e7c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





## 正常显示全屏-DecorView 设置最小宽度和 padding 为 0

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
//window.getDecorView().setBackgroundColor(Color.GREEN);
```



![图片描述](http://upload-images.jianshu.io/upload_images/6737388-05503b9a1f680b99.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[图片上传失败...(image-246532-1540983998175)]


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-46665f7b4b02b344.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[图片上传失败...(image-46f0c1-1540983998175)]

就像上面这个图片一样，其实红色框就是 TextView ，有一部分没有显示出来。

## 正常显示全屏-DecorView 设置最小宽度

```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
//window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
//window.getDecorView().setBackgroundColor(Color.GREEN);
```

![图片描述](http://upload-images.jianshu.io/upload_images/6737388-52825148251b5981.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[图片上传失败...(image-98e5b4-1540983998175)]





## 正常显示全屏-DecorView padding 为 0



```java
DialogUtils.show(dialogMyAddress, Gravity.BOTTOM);
Window window = dialogMyAddress.getWindow();
Window window1 = getWindow();
window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
layoutParams.horizontalMargin = 0;
window.setAttributes(layoutParams);
//window.getDecorView().setMinimumWidth(getResources().getDisplayMetrics().widthPixels);
//window.getDecorView().setBackgroundColor(Color.GREEN);
```

**结果和设置最小宽度和 padding 为 0 是一样的**


![图片描述](http://upload-images.jianshu.io/upload_images/6737388-6bc870d045da896d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[图片上传失败...(image-db924a-1540983998175)]







## 总结

其实要想设置 Dialog  宽度占满全屏很简单，掌握了原理就可以了。原理分析：通过上面的实验，我们可以了解到一个 Dialog 布局，最底层是 DecorView 这个底层布局是有一个默认的 padding 的，并且它有默认大小，宽度并不是占满屏的。这就导致了我们单独设置了 Window 的 LayoutParams 为 MATCH_PARENT 的时候始终是不能占据满屏的。这是因为 DecorView 的大小限制了 Window。因此我们需要在设置 Window 大小的前面，设置 DecorView 的大小 `window.getDecorView().setLayoutParams(new WindowManager.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT,500));` 你也可以这样设置，但是这样设置实际上是占满屏了，但是别忘了 DecorView 的 padding ，这样最外面一圈始终有 padding 看上去的效果就是还是没有占满屏。

所有最简单的方法还是

```java
Window window = dialogMyAddress.getWindow();
// 把 DecorView 的默认 padding 取消，同时 DecorView 的默认大小也会取消
window.getDecorView().setPadding(0, 0, 0, 0);
WindowManager.LayoutParams layoutParams = window.getAttributes();
// 设置宽度
layoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
window.setAttributes(layoutParams);
// 给 DecorView 设置背景颜色，很重要，不然导致 Dialog 内容显示不全，有一部分内容会充当 padding，上面例子有举出
window.getDecorView().setBackgroundColor(Color.GREEN);
DialogUtils.show(dialogMyAddress);
// 注意代码的顺序
```
