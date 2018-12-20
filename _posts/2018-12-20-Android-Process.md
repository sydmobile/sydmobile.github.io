---
layout: post
title:  "Android 中进程的级别以及 Service 的优先级"
date:   2018-12-20 15:14:54
categories: Android
tags: android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}













# Android 中进程的级别以及 Service 的优先级

由于在 bindService 启动 service 的时候需要传入 flag （可以看一下这篇文章）[Service](https://www.jianshu.com/p/d88020de60b1) ，这里有介绍 flag 的作用，和启动的 Service 的优先级有关系，一般传入：`BIND_AOUT_CREATE`，这里我们需要了解一下 Android 中进程的优先级的情况。

进程的五个常用级别：

- **前台进程**（Foreground process）：前台进程就是用户当前要处理的所有事情都必须要使用的进程。满足下面的各种情况则认为是前台进程。

  - 进程持有一个正在和用户交互的 Activity。
  - 进程持有一个 Service，这个 Service 处于这几种状态：1. Service 与用户正在交互的 Activity 绑定。 2. Service 是在前台运行的。 3. Service 正在执行它的生命周期 onCreate() onStrarCommand，onDestroy。 4. 进程持有一个 BroadcastReceiver 这个 BroadcastReceiver 正在执行它的 onReceiver 方法。

  **杀死前台进程需要用户交互，前台进程的优先级最高**

- **可见进程**（Visible process）：如果一个进程不含任何前台的组件，但仍可被用户在屏幕上看到。当满足下面任意一条的时候，进程被认为是可见的。

  - 进程持有一个 activity，这个 activity 不在前台。但是仍然可见的情况。
  - 进程持有一个 Service ，这个 Service 与一个可见的 Activity 绑定。

  **可见的进程也被认为很重要，一般不会被销毁，除非是为了保证所有前台进程的运行而不得已不杀死可见进程的时候**

- **服务进程**（Service process）：如果一个进程中运行着一个 Service，这个 Service 是通过 startService() 开启的，并且不属于上面两种较高优先级的情况下，这个进程就是一个服务进程。尽管服务进程没有和用户可以看到的东西绑定，但是它们一般在做的事情是用户关心的，比如后台播放音乐，后台下载数据等。**所以系统会尽量维持它们的运行，除非系统内存不足以维持前台进程和可见进程的运行需要**（这句话和没说一样）

- **后台进程**（Background process）：如果进程不属于上面三种情况，但是进程持有一个用户不可见的 activity （activity 的 onStop 被调用，但是 onDestroy 没有被调用的状态）就认为进程是一个后台进程。

  **后台进程不直接影响用户体验，系统会为了前台进程、可见进程、服务进程而任意杀死后台进程**，通常情况下会有很多后台进程存在，他们会被保存在一个 LRU（least recently used）列表中，这样就可以确保用户最近使用的 Activity 最后被销毁，先销毁时间最远的 Activity。

- **空进程**：如果一个进程不包含任何活跃的应用组件，则认为是空进程。例如：一个进程当中已经没有数据运行了，但是内存当中还为这个应用保留了一个进程空间。保存这种进程的唯一理由是为了缓存的需要，为了加快下次启动这个进程中组件的启动时间，这种空进程经常被杀死。

**总结：** 我们已经知道有这 5 个进程了，并且他们的优先级都列出来的，这样我们就可以根据优先级来让我们的 APP 尽量不被杀死了。

下面再来说一下 BindService() 方法中的 flag 参数：

- BIND_AUOT_CREATE:

  只要绑定存在就会自动创建这个 Service，虽然创建了 Service，但是它的 onStartCommand 方法是不会调用的，因为这个方法只有在 startService 的时候被调用。

  在 Android 4.0 以前，不提供这个标志的话，会影响系统判定当前 Service 进程的重要性（会把它认为是后台进程），当要设置的时候，告诉系统进程重要性的唯一方式是，通过 bindService 来实现，在这种情况下，只有 Activity 在前台才会起作用。（这样 Service 进程的优先级等同于启动它的进程的优先级）。

  现在要想把 Service 进程的优先级降低，必须提供新的 falg （BIND_ADJUST_WITH_ACTIVITY）。考虑到兼容性，如果没有指定 BIND_AUTO_CREATE 的时候，系统会自动加上 BIND_WAIVE_PRIORITY 和 BIND_ADJUST_WITH_ACTIVITY 来实现降低优先级的效果。因为在 Android 4.0 以前 Service 的优先级默认是后台进程，在 Android 4.0 之后默认是等同于宿主进程，所以只有设置了 BIND_WAIVE_PRIORITY 后才会 4.0 和 4.0以前都兼容起来被当做后台任务对待。

- BIND_DEBUG_UNBIND:

  用来 debug 使用的

- BIND_NOT_FOREGROUND

  不允许将绑定的 Service 的进程提升到前台进程的优先级，它将仍然拥有和客户端同样的内存优先级，所以在宿主进程没有被杀死的情况下，Service 的进程也是不会被杀死的。但是 cpu 可能会把它放在后台执行。仅仅在这种情况下会有作用，宿主进程在前端，Service 进程在后台

- BIND_ABOVE_CLIENT

  在这种情况下，Service 进程比 App 本身的进程还有重要，当设置后，内存溢出的时候，将会在关闭 Service 进程前关闭 App 进程。但是这种情况不能保证。

- BIND_ALLOW_OOM_MANAGEMENT

  允许内存管理系统管理 Service 的进程，允许系统在内存不足的时候删除这个进程。

- BIND_WAIVE_PRIORITY

  不影响 Service 进程的优先级的情况下，允许 Service 进程被加入后台队列中。

- BIND_IMPORITANT

  这个服务对于这个客户端来说是非常重要的，所以应该提升到前台进程的级别。一般这个进程
  会提升到可见的级别，甚至客户端在后台的时候。

- BIND_ADJUST_WITH_ACTIVITY

  如果从一个 Activity 绑定，则这个 Service 进程的优先级和 Activity 是否对用户可见有关。



好了，就介绍到这里，由于个人水平有限，有错误的地方，请各位不吝赐教！

由于个人精力有限，多个渠道发布，排版可能会有问题。重点维护  **Android开发家园**

![Android 开发者家园](http://upload-images.jianshu.io/upload_images/6737388-cd206f87b3b3edc6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

