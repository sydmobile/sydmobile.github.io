---
layout: post
title:  "Android Service (上)"
date:   2018-12-18 15:14:54
categories: Android
tags: android service
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}










# Android Service 详解（上）

Service 作为 Android 的四大组件还是很有必要好好掌握一下的！

## Service 生命周期

先从 Service 生命周期看起，Service 的生命周期比较有趣的一点是，它的生命周期会根据调用不同的方法启动有不同的表现，具体有两种形式。

1. 通过 startService(Intent intent) 启动 Service 

   生命周期是这样的： `onCreate()`  、`onStartCommand()`、`onStart()(已经过时)` 、`onDestroy()`   

2. 通过 bindService(Intent intent,ServiceConnection conn,int flags) 启动 Service

   生命周期是这样的：`bindService()`、`onCreate()` 、`IBinder onBind(Intent intent)`、`unBindService()`、`onDestroy()` 方法。

### 通过 startService 启动

前面说了，通过 `startService()` 启动 Service 会执行 `onCreate()`、`onStartCommand()`、`onDestroy()` 方法。注意：当调用 startService(Intent intent) 后，之后只执行一次 `onCreate()` 方法，反复多次调用 startService 后，Service 只会重复执行 `onStartCommand` `onStart()(已经过时)` 方法。调用 stopService 后 Service 只执行一次 `onDestroy` 方法。  

可以看到通过这种方式启动 Service ，这个时候的 Service 几乎和 Activity 不能交互（不考虑全局变量的方式），在 Service 里面也没有 `getIntent()` 方法。

### 通过 bindService 启动

既然通过 startService 启动的 Service 和 Activity 没有建立联系，那么通过 bindService 来启动 Service，就可以和 Activity 建立联系了，相当于 Service 绑定到了这个 Activity 中了。

通过 bindService(Intent intent ,ServiceConnection connetion,int flag) 启动 Service 后 Service 的正常的生命周期是：onCreate、onBind、ibinder 会作为参数传递到 connect 中的 onServiceConnected 方法中，绑定后，再次执行 bindService Service 什么都不执行。 

Activity 在没有 bindService 的情况下，调用 unBindService(ServiceConnection serviceConnection) 是会 crash 的。无论在什么情况下，对于某个 Activity，只能够执行一次 unBindService。因为执行完一次 Service就不再注册 serviceConnection 了，再次 unBinderService 就会出现错误 `Service not registered:MainActivity$ServiceConnection$`  只有在单独执行 bindService （执行 bindService 前后没有执行 startService）的情况下，执行 unBindService 才会正常执行 Service 的 onDestroy 方法。

宿主 Activity 退出，不会正常执行 Service 的 onDestroy 方法。

#### 关于 binSerive(Intent intent,ServiceConnection connection,int flag) 中的参数

- 第一个参数就是要绑定的 Service 的intent 就不多说了
- 第二个参数就是 Service 和 Activity 建立联系使用的
- 标志位，和启动的 Service 的优先级有关，一般就是传入：`BIND_AOUT_CREATE` 表示在 Activity 和 Service建立关联后自动创建 Service。

其实上面的标志位目前作用不大，这个阶段大可不必细究。但是为了满足心理需要，我还是大体解释一下可以使用的参数。(放到下一篇吧)

### 关于 Service 启动总结

上面介绍的都是在单独执行 startService 或者 binService 的情况下。

当既执行了 startService 又执行了 binService 的时候（不区分顺序），只有执行过 stopService 和unBindService 的时候（不区分顺序）才执行 onDestroy 方法。

**任何一个 Service 在整个应用程序范围内都是通用的**。即 MyService 不仅可以和 MainActivity 建立关联，还可以和任何一个 Activity 建立关联。如果 Service 和多个 Activity 绑定，则只有这个 Service 与 所有 Activity 接触绑定后，才会执行 onDestroy 方法。（如果 Activity 被销毁了，也认为 Service 和这个 Activity 解除了绑定）如果在某个 Activity 调用 startService 后，Service 就被启动了，这个时候，无论你在哪个 Activity 内再调用 startService 效果就和上面分析的多次调用 Service 效果是一样的。  

以上是简单概念，还有下篇。

![欢迎大家关注我的微信公众号，和我交流分享](http://upload-images.jianshu.io/upload_images/6737388-1eca35c3d7e04a1e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

