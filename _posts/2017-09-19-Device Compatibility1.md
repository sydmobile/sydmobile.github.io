---
layout: post    
title:  "Android设备兼容性 1"
date:   2017-09-19 22:14:54
categories: Android官方翻译
tags: 翻译
excerpt: Android官方文档翻译
mathjax: true
author: sydMobile
---
* content
{:toc}








### Device Compatibility
### 设备兼容性

Android is designed to run on many different types of devices, from phones to tablets and televisions.Android被设计为可以运行在许多不同的设备上面，从手机到平板 再到电视机。 As a developer, 作为一名开发者the range of devices provides a huge potential audience for your app.设备的范围为您的应用程序提供了巨大的潜在受众。 In order for your app to be successful on all these devices,为了使您的应用程序在所有这些设备上成功 it should tolerate some feature variability and provide a flexible user interface that adapts to different screen configurations.它应该有一些 变化的特点，并提供适应不同屏幕配置的灵活的用户界面。

To facilitate your effort toward that goal,为了方便你能实现这些目标 Android provides a dynamic app framework in which you can provide configuration-specific app resources in static files (such as different XML layouts for different screen sizes).Android提供了一个动态应用程序框架，您可以在其中为静态文件提供配置特定的应用程序资源（这里是一个链接介绍app resource）（比如：为不同的屏幕尺寸配置不同的xml布局） Android then loads the appropriate resources based on the current device configuration.Android会根据当前设备的配置选择 载入合适的资源 So with some forethought to your app design and some additional app resources, you can publish a single application package (APK) that provides an optimized user experience on a variety of devices.所以在设计你的app的时候需要有前瞻，加入一些合适的资源，你就可以仅仅通过发布一个apk在不同的设备上获取很好的用户体验

If necessary, however, you can specify your app's feature requirements and control which types of devices can install your app from Google Play Store. This page explains how you can control which devices have access to your apps, and how to prepare your apps to make sure they reach the right audience. For more information about how you can make your app adapt to different devices, read Supporting Different Devices.

不过，如有需要，您可以指定应用程式的功能要求，并控制哪些类型的装置可以从Google Play商店安装应用程式。 本页说明如何控制哪些设备可以访问您的应用，以及如何准备应用程序，以确保他们能够接触到正确的受众群体。 有关如何使应用适应不同设备的更多信息，请参阅支持不同的设备。

What Does "Compatibility" Mean? 兼容性的意思是什么？
As you read more about Android development, you'll probably encounter the term "compatibility" in various situations. There are two types of compatibility: device compatibility and app compatibility.

当您阅读有关Android开发的更多信息时，您可能会在各种情况下遇到“兼容性”一词。 兼容性有两种：设备兼容性（设备的兼容性指的是各个制造商制作的搭乘Android系统的设备是否是符合Android系统兼容性的）和应用程序兼容性（这里 就是指的我们开发的APP）。

Because Android is an open source project, any hardware manufacturer can build a device that runs the Android operating system. Yet, a device is "Android compatible" only if it can correctly run apps written for the Android execution environment. The exact details of the Android execution environment are defined by the Android compatibility program and each device must pass the Compatibility Test Suite (CTS) in order to be considered compatible.

由于Android是一个开源项目，任何硬件制造商都可以构建运行Android操作系统的设备。但是，一个设备只有可以正确的运行一个用Android编写的可执行APP才可以称为“Android兼容”Android执行环境的具体细节由Android兼容性程序定义（Android兼容性这里指的设备制造商制作的搭乘Android系统的 手机，比如小米、华为这种手机制造商。什么样的手机才叫做Android系统，可以运行我们开发的Android 程序这个谷歌是有一个标准的 https://source.android.com/compatibility/overview 这里有详细的介绍这是针对手机制造商来说的，比如华为的 EMUI 是搭乘Android系统的，EMUI必须符合谷歌罗列的这些标准才算是一个Android兼容设备），每个设备必须通过兼容性测试套件（CTS）才能被认为兼容。

As an app developer, you don't need to worry about whether a device is Android compatible, because only devices that are Android compatible include Google Play Store. So you can rest assured that users who install your app from Google Play Store are using an Android compatible device.

作为应用开发者，您不必担心设备是否与 Android 兼容，因为只有Android兼容的设备才包括Google Play商店。 因此，您可以放心，从 Google Play 商店安装您的应用的用户正在使用Android兼容设备。这里的主要意思就是我们不要担心我们开发的APP会运行在不是Android系统的设备上面，因为只要用户可以在Google Play中下载了我们的APP，就说明用户的这个设备就是兼容Android的。

However, you do need to consider whether your app is compatible with each potential device configuration. Because Android runs on a wide range of device configurations, some features are not available on all devices. For example, some devices may not include a compass sensor. If your app's core functionality requires the use of a compass sensor, then your app is compatible only with devices that include a compass sensor.

但是，您需要考虑您的应用程序是否与每个潜在的设备配置兼容。 由于Android运行在各种设备配置上，因此某些功能在所有设备上都不可用。 例如，某些设备可能不包括罗盘传感器。 如果您的应用程序的核心功能需要使用罗盘传感器，那么您的应用程序只能与包含罗盘传感器的设备兼容。  


![扫一扫关注微信公众号，获取更多干货和资源](https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png)  
扫一扫关注微信公众号，获取更多干货和资源。
