---
layout: post
title:  "Android 入门--Activity"
date:   2018-12-23 15:14:54
categories: Android
tags: android
excerpt: Activity 介绍
mathjax: true
author: sydMobile
---
* content
{:toc}











## Activity 是什么

简单的来说，一个 Activity 包含了用户可以看到的界面，用来和用户进行交互。一个应用程序中可以有零个或者多个 Activity。零个 Activity 的话就是，这个程序不包含与用户交互的界面。

## 返回栈（任务栈）（任务）

Android 是使用任务（Task）来管理 Activity 的。一个 Task 就是一组存放在栈里的 Activity 的集合。这个任务也被称为返回栈（Back Stack），栈是一种先进先出的数据结构，默认情况下，每当我们启动了一个新的 Activity，它会被加入到栈中，并处于栈顶的位置，当 Activity 的 finish 方法去销毁一个 Activity 的时候，这个 Activity 就会出栈。系统总是会显示处于栈顶的 Activity 给用户。

## Activity 的状态

每个 Activity 在其生命周期中最多可能会有 4 中状态

- 运行状态

  当一个 Activity 位于返回栈的栈顶的时候，这个时候这个 Activity 就处于运行状态。这个时候是最不容易被系统回收的。

- 暂停状态

  当一个 Activity 不再处于栈顶位置，但是仍然可见的时候，这种情况下这个 Activity 就处于暂停状态。Activity 不在栈顶了竟然还能被看到？你想如果栈顶的 Activity 是一个对话框或者是透明的情况下，是不是这个不在栈顶位置的 Activity 依然可以被用户看到啊。这种状态的 Activity，系统也是不愿意去回收的。

- 停止状态

  当一个 Activity 不再处于栈顶位置，而且完全看不到的时候，就进入了停止状态了。系统仍然会为这样的 Activity 保留响应的状态和成员变量，但这并不是完全可靠的，当内存紧张的时候，处于停止状态的 Activity 有可能被系统回收。

- 销毁状态

  当一个 Activity 从返回栈移除后就变成销毁状态了，系统倾向回收处于这种状态的 Activity。

## Activity 的生命周期

先放上一张最经典的图

![](https://upload-images.jianshu.io/upload_images/6737388-1156add8ffb9d006.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- onCreate()  

  会在 Activity 第一次被创建的时候调用，应该在这个方法中完成 Activity 的初始化操作，比如加载布局、绑定事件等等。

- onStart() 

  Activity 由不可见状态变为可见状态。

- onResume()

  这个方法在 Activity 准备好和用户进行交互的时候调用，此时 **Activity 一定处于返回栈的栈顶，并且处于运行状态**。

- onPause() 

  这个方法在系统准备去启动或者恢复另一个 Activity 的时候调用。通常会在这个方法中将一些消耗 CPU 的资源释放，保存一些关键的数据，但是这个方法的执行速度一定要快，不然会影响新的栈顶 Activity 的使用。

- onStop()

  在 Activity 完全不可见的时候调用。

- onDestroy()

  这个方法在被销毁之前调用，之后 Activity 的状态就变为销毁状态。

- onRestart() 

  这个方法在 Activity 由停止状态变为运行状态之前调用，也就是 Activity 被重新启动了。

以上 7 个方法中除了 onRestart 方法，其他都是两辆相对的，从而又可以将 Activity 分为 3 中生存期。

- **完整生存期：**Activity 在 onCreate 和 onDestroy 方法之间所经历的，就是完整的生存期。一般情况下，我们需要在 onCreate 方法中完成各种初始化操作，在 onDestroy 中完成释放内存的操作。
- **可见生存期：**Activity 在 onStart 和 onStop 之间所经历的，就是可见生存期。在可见生存期，Activity 对于用户总是可见的，但是可能无法和用户交互。我们可以通过这两个方法来合理的管理那些对用户可见的资源。在 onStart 方法中对资源进行加载，在 onStop 方法中对资源进行释放，从而使得处于停止状态的 Activity 不会占用太多内存。
- **前台生存期：**Activity 在 onResume 和 onPause 方法之间所经历的就是前台生存期。此时的 Activity 是可以和用户进行交互的。

### 特别情况下的生命周期

上面都是正常情况的下面生命周期，下面来几个比较有特点的生命周期。

- Activity  A 启动后，启动一个正常的 Activity B，这时 A 的生命周期：`onCreate ->onStart -> onResume -> onPause -> onStop -> B 的生命周期`。
- Activity  A 启动后，启动一个 Dialog 样式或者透明的 Activity B，这时 A 的生命周期 `onCreate ->onStart -> onResume -> onPause -> B 的生命周期`。
- 锁屏状态和按下 HOME 键的时候 Activity 都是会执行 onStop 的
- 旋转屏幕的情况：Activity 的生命周期是会重新开始的。比如当前 Activity 处于竖屏，然后旋转成横屏，这个时候 Activity 会执行 onPause  onStop  onDestroy，由于是在异常情况下终止的，系统会调用 onSaveInstance 来保存当前 Activity 的状态。
- Activity 先回调 onResume 后 onCreateOptionsMenu



### Activity 意外终止

Activity 进入停止状态后，就有可能被系统回收。Activity 为我们提供了一个方法 `onSaveInstanceState()` 回调方法，这个方法可以保证在 Activity 被回收之前一定会被调用，我们可以通过这个方法来保存一些临时数据。

## Activity 的启动模式

启动模式一共有 4 中：standard、singleTop、singTask 和 singleInstance，可以在 `AndroidManifest.xml` 中通过 `<activity>` 标签指定 `android:launchMode` 属性来选择启动模式。

### standard 模式

standard 模式是 Activity 的默认启动模式，在不进行显示指定的情况下，所有 Activity 都会自动使用这种启动模式。这种模式下，每当启动一个新的 Activity，它就会在返回栈的栈顶位置。对于使用 standard 模式启动的 Activity，系统不会在乎这个 Activity 是否已经存在在栈中了。每次启动的时候都会创建一个新的实例。

谁启用了这个模式的 Activity，那么这个 Activity 就属于启动它的 Activity 的任务栈。

### singleTop 模式

如果 Activity 指定为 singleTop，在启动 Activity 的时候发现返回栈的栈顶已经是该 Activity 了。则认为可以直接使用它，就不会再创建新的 Activity 实例了。

因为不会创建新的 Activity 实例，所以 Activity 的生命周期就没有什么变化了。但是它的 `onNewIntent`  方法会被调用。

### singleTask 模式

singleTop 很好的解决了重复创建栈顶 Activity 的问题。如果 Activity 没有处于栈顶的位置，还是可能会创建多个 Activity 实例的。如何解决这种问题呢？那就需要借助 singleTask 了。当 Activity 的启动模式为 singleTask 的时候，每次启动该 Activity 的时候系统会首先在返回栈中检查是否存在该 Activity 的实例，如果发现已经存在则直接使用该实例。并把这个 Activity 之上的所有 Activity 全部出栈，如果没有就会创建一个新的 Activity 实例。

生命周期正常调用，onNewIntent 也会被调用。

### singleInstance 模式

singleInstance 模式的 Activity 会启用一个新的返回栈来管理这个 Activity （其实如果 singleTask 模式指定了不同的 taskAffinity，也会启动一个新的返回栈）。意义：假如我们的程序中有一个 Activity 是允许其他程序调用的，如果我们想实现其他程序和我们的程序可以共享这个 Activity 实例。那么如何实现呢？假如使用前面 3 中启动模式，肯定不行。因为，我们每个应用程序都有自己的返回栈，虽然是同样这个 Activity，但是在不同的返回栈入栈的时候肯定是创建了新的实例了。而 singleInstance 可以解决这个问题，在这种模式下会有一个单独的返回栈来管理这个 Activity，不管是那个应用程序来访问这个 Activity，都共用的同一个返回栈，也就解决了共享 Activity 实例的问题。

### 总结

standard 和 singleTop 启动模式都是在原任务栈中新建 Activity 实例，不会启动新的 Task，即使你指定了 taskAffinity 属性。

什么是 taskAffinity 属性

- 标识了一个 Activity 所需任务栈的名字，但不能根据这个名字来确定任务栈。因为你可以理解同样一个任务栈有多个名字。默认情况下，所有 Activity 所需的任务栈的名字为应用的包名。
- 可以单独指定每一个 Activity 的 taskAffinity 属性覆盖默认值
- 一个任务的 affinity 决定于这个任务的根 Activity 的 taskAffinity
- 在概念上，具有相同的 affinity 的 activity （设置了相同 taskAffinity 属性的 activity）属于同一个任务。
- 为一个 activity 的 taskAffinity 设置一个空字符串，表明这个 Activity 不属于任何 task

**taskAffinity 属性不对 standard 和 singleTop 模式有任何影响** 即使我们给这两种模式设置了

```xml
<activity android:name=".ActivityStandard" android:launchMode="standard" android:taskAffinity="com.syd"/>
```

只是改变了 taskAffinity 的名字，但是任务栈依然还是启动这个 Activity 的 Activity 所在的任务栈。



singleTask 模式的 Activity 的启动，启动的时候会根据 taskAffinity 去寻找当前是否存在这么一个任务栈，如果不存在就会创建一个新的任务栈，并创建新的 Activity 实例到进创建的任务栈中。如果存在，则查找任务栈中是否有该 Activity 的实例。

任务栈是可以跨程序的。

> 参考：《第一行代码》、https://blog.csdn.net/mynameishuangshuai/article/details/51491074






