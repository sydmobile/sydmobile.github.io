---
layout: post
title:  "Android设备兼容性 2"
date:   2017-08-29 15:14:54
categories: Android官方翻译
tags: Android官方翻译
excerpt: Android官方翻译
mathjax: true
author: sydMobile
---
* content
{:toc}






> 文章最早发布于我的微信公众号中，欢迎大家扫描下面二维码关注微信公众获取更多干货资源。

本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

### Controlling Your App's Availability to Devices
### 控制你的程序的可用性在不同的设备上

Android supports a variety of features your app can leverage through platform APIs. Some features are hardware-based (such as a compass sensor), some are software-based (such as app widgets), and some are dependent on the platform version. Not every device supports every feature, so you may need to control your app's availability to devices based on your app's required features.

Android支持许多的功能，你的APP可以通过API充分的利用这些功能。一些功能是硬件作为支持（比如指南针传感器），一些是软件作为支持（比如APP小空间），还有一些依赖于平台的版本（也就是Android平台的版本）不是每个设备都支持Android的所以功能。所以你需要根据你的app所需要的特点来控制你的APP的实现。

To achieve the largest user-base possible for your app, you should strive to support as many device configurations as possible using a single APK. In most situations, you can do so by disabling optional features at runtime and providing app resources with alternatives for different configurations (such as different layouts for different screen sizes). If necessary, however, you can restrict your app's availability to devices through Google Play Store based on the following device characteristics:

为了让你的APP实现最大的用户量，你应该努力的去支持大量的设备通过用一个APK，在大多数情况下，你可以舍弃一些不必要的功能提供一些在不同设备上必须的资源（比如为不同的屏幕尺寸添加不同的布局文件，这就是屏幕适配的一种方式）如果有必要的话，你也可以通过Google Play来限制一些不可以使用的设备。

### Device features
### 设备特点

In order for you to manage your app’s availability based on device features, Android defines feature IDs for any hardware or software feature that may not be available on all devices. For instance, the feature ID for the compass sensor is FEATURE_SENSOR_COMPASS and the feature ID for app widgets is FEATURE_APP_WIDGETS.

为了让您根据设备功能来管理应用的可用性，Android定义了所有设备可能无法使用的任何硬件或软件功能的功能ID。 例如，罗盘传感器的功能ID为FEATURE_SENSOR_COMPASS，应用程序小部件的功能ID为FEATURE_APP_WIDGETS。

If necessary, you can prevent users from installing your app when their devices don't provide a given feature by declaring it with a <uses-feature> element in your app's manifest file.

如果需要，您可以通过在应用程序的清单文件中使用<uses-feature>元素声明该设备，从而避免用户安装应用程序。

For example, if your app does not make sense on a device that lacks a compass sensor, you can declare the compass sensor as required with the following manifest tag:

```
<manifest ... >
    <uses-feature android:name="android.hardware.sensor.compass"
                  android:required="true" />
    ...
</manifest> 
``` 


例如，如果您的应用在缺少罗盘传感器的设备上无效，则可以根据需要使用以下清单标签声明罗盘传感器：

Google Play Store compares the features your app requires to the features available on each user's device to determine whether your app is compatible with each device. If the device does not provide all the features your app requires, the user cannot install your app.

Google Play商店将您的应用所需的功能与每个用户设备上的功能进行比较，以确定您的应用是否与每个设备兼容。 如果设备没有提供您的应用所需的所有功能，则用户无法安装您的应用。

However, if your app's primary functionality does not require a device feature, you should set the required attribute to "false" and check for the device feature at runtime. If the app feature is not available on the current device, gracefully degrade the corresponding app feature. For example, you can query whether a feature is available by calling hasSystemFeature() like this:
```
PackageManager pm = getPackageManager();
if (!pm.hasSystemFeature(PackageManager.FEATURE_SENSOR_COMPASS)) {
    // This device does not have a compass, turn off the compass feature
    disableCompassFeature();
}
```
然而，如果您的应用程序的主要功能不需要设备功能，则应将所需属性设置为“false”，并在运行时检查设备功能。 如果应用功能在当前设备上不可用，则会适当地降低相应的应用功能。 例如，您可以通过调用hasSystemFeature（）来查询功能是否可用：

For information about all the filters you can use to control the availability of your app to users through Google Play Store, see the Filters on Google Play document.

有关所有过滤器的信息，可以通过Google Play商店来控制用户的应用程序的可用性，请参阅Google Play上的过滤器文档。

Note: Some system permissions implicitly require the availability of a device feature. For example, if your app requests permission to access to BLUETOOTH, this implicitly requires the FEATURE_BLUETOOTH device feature. You can disable filtering based on this feature and make your app available to devices without Bluetooth by setting the required attribute to "false" in the <uses-feature> tag. For more information about implicitly required device features, read Permissions that Imply Feature Requirements.

注意：某些系统权限隐含地要求设备功能的可用性。 例如，如果您的应用程序请求访问BLUETOOTH的权限，则会隐式地需要FEATURE_BLUETOOTH设备功能。 您可以根据此功能禁用过滤，并通过在<uses-feature>标签中将所需属性设置为“false”，使您的应用程序可用于无蓝牙设备。 有关隐式需要的设备功能的更多信息，请阅读具有要求功能要求的权限。

未完待续....
<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  扫一扫关注微信公众号，获取更多干货和资源。
</p>
