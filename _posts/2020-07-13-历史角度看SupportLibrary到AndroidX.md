---
layout: post
title:  "历史角度看SupportLibrary到AndroidX"
date:   2020-07-13 22:14:54
categories: Android
tags: Androidx
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}


















![](https://user-gold-cdn.xitu.io/2019/7/3/16bb8261cc0dca97?w=1080&h=237&f=png&s=1025845)
[更多文章分类](https://mp.weixin.qq.com/s/B8DP0UMg1fup2_sJVtgjMw)  

我们都知道 Google 在 2014 年 I/O 大会上为了统一我们 Android 端 APP 的设计风格，让 APP 更加美观，发布了新的设计语言----Material Design。突出“卡片设计”。基于网格的布局、响应动画与过渡、填充、深度效果（如光线和阴影）。

它是一种设计规范，是设计人员应该去学习的，无关乎用什么开发语言，大家不要搞混了！

推出 `Material Design` 后，Google 在 Android 5.0 上将自家的所有内置应用都使用了 `Material Design` 的风格来进行设计。样式非常美观。

![](https://user-gold-cdn.xitu.io/2020/7/13/173479db953aebae?w=2134&h=1600&f=jpeg&s=231700)

![](https://user-gold-cdn.xitu.io/2020/7/13/173479e02f42cd7d?w=1600&h=1561&f=png&s=1208640)

虽然样式非常美观，但是推出后普及程度非常不理想，特别是在中国，由于 `MaterialDesign` 只是一个设计规范，主要是面向 UI 设计师的，UI 设计师应该去学习这种设计风格，然后设计出属于这种官方推荐的风格的 APP。我们都知道国内的特殊情况，Google 一直进不来，手机厂商又有许多，而且彼此不统一，没有一个很好的管理者，因此碎片化十分严重，想要使用 `Material Design` 这种设计风格来统一所有的 android APP 那几乎不可能。就现在而言，你问一个 UI 设计师什么是 `Material Design` 他们可能都不知道，只知道照搬 iOS 上的 APP 的设计风格然后抄一遍。这里说的只是国内的情况。

当然就算你的 UI 设计师真正懂了 `Material Design` 出了原型图了，那么对于开发者人员来说自己去实现 `Material Design` 的效果也是很难的。于是 Google 为了解决这个问题在 2015 年的 I/O  大会上推出了 `Design Support` 库，在这个库将 `Material Design` 中一些代表性的控件和效果进行了封装，来帮助开发者完成一个属于 `Material Design`  设置风格的 APP 

好了，到此为止 `Material Design` 的一段历史就介绍完了，下面开始讲下一段历史了。

我们都知道 Android 在 2008 年发布了它的第一个正式版本，系统发布后都是要不断的进行迭代更新的，新的系统中会加入新的 API，但是这些新加入的 API 在老版本的系统中是没有的，这个时候如果我们的 APP 中使用了新版本中加入的 API，那么运行在新版本系统的手机上是可以的，如果在低版本的手机上就会出问题了，为了兼容低版本手机。推出了 `Android Support Library` 库，一些后来添加的 api或者补充的内容都会放到 `support` 库中，注意 `support` 库不是一个库，它也有多个拆分，按需引入就可以了。比如，如果你需要上面的 `Material Design` 一些风格的库，就可以引入 `com.android.support:design` 这个库，这个里面包括了所有与 `Material Design` 相关的控件内容。当然我还可以单独引入具体的某个控件。再比如：`support-v4`  `supoprt-v7`  这些库都是属于 `Android Support Library` 库的。

最初的时候 `v4`  `v7`  这些数字都是表示系统可以兼容到 `api` 版本多少，比如 `v4`  表示可以兼容到 `api 4`  对应的 Android 系统版本就是 1.6 。现如今这些早已过时了，从支持库版本 26.0.0 (2017年7月)开始，对于大多数库软件包支持的最低 API 级别已经提升到 Android 4.0（API 14）了。所以 `v4` 这个数字的意义也不是原先的意义了。关于支持库的更多内容：https://developer.android.com/topic/libraries/support-library?hl=zh-cn#api-versions

需要注意一点的是支持库也是有对应的版本号的


![](https://user-gold-cdn.xitu.io/2020/7/13/173479ec26f3d13f?w=275&h=560&f=png&s=37197)

一般添加支持库的时候格式都是这样的 `implementation 'com.android.support:xxxx:版本号'`

比如：

```
implementation 'com.android.support:design:28.0.0'
implementation 'com.android.support:appcompat-v7:26.1.0'
```

为了解决 `support` 上面的问题，在 2018 Google I/O 大会上推出了 AndroidX 来替换了 `Android Support Library`  。在 Android 9.0 （API级别 28） 正式发布后，新版本的支持库 `AndroidX` 就诞生了。它属于 Jetpack，除了现有的支持库以外，AndroidX 库还包含了最新的 JetPack 组件，在 Api27及更早版本，依然可以使用 `Support Library` 但是之后新开发的所有库都将在 AndroidX 库中进行了。

因此 AndroidX 库是 `Support Library` 库的替换，在 API 28 及以后就要使用 AndroidX 库来彻底替换 `Support Library` 库了。**注意千万不要两者都出现，一定要做到统一**  这里重磅推出 `com.google.android.material:material:1.1.0` 这个库，这个库就对应了我们上面介绍的 `Material Design` 的 `design`  支持库了，为什么要单独强调这个库呢！因为这个新加的库太强大了！可以认为是 `design`  库的升级版，里面的控件比之前`design`中的使用起来更加的顺手！好了上面介绍那么多主要是为了引入这个库的。之后会详细来说 `material:material` 这个库的！

![](https://user-gold-cdn.xitu.io/2020/7/13/173479f1c5f1bae8?w=1274&h=1214&f=png&s=168541)

AndroidX 和 原先的 `Support Library` 都有对应的关系如上图，具体看：https://developer.android.com/jetpack/androidx/migrate/artifact-mappings

关于 AndroidX 的版本号查看：https://developer.android.com/jetpack/androidx/versions

最后注意：**如果你的项目的 compileSdkVersion 是28的话，支持库就别再用 Support Library 了，要换成 AndroidX**   **重中之重的是如果换成了 AndroidX 依赖后，里面就千万不要再出现 support 这样的库了**

![](https://user-gold-cdn.xitu.io/2019/10/10/16db5064ec1e7b6d?w=1240&h=620&f=jpeg&s=145465)

