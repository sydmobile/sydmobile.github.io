---
layout: post
title:  "从0系统学Android-2.3使用 Intent 在 Activity 之间穿梭"
date:   2019-07-23 15:14:54
categories: 从0系统学Android
tags: Android 
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}














![f](https://github.com/sydmobile/sydmobile.github.io/blob/master/pic/%E5%A4%B4%E5%9B%BE%E7%89%87%E4%B8%A2%E5%A4%B1.png?raw=true)

### 2.3 使用 Intent 在 Activity 之间穿梭

在上一节中我们已经学会了如何创建一个 Activity 了。对于一个应用程序来说，肯定不可能只有一个 Activity。下面就来学习多个 Activity 是专门跳转的。

#### 2.3.1 使用显式 Intent

对于创建 Activity 的过程我们已经很熟悉了，下面快速的创建第二个 Activity。取名 `SecondActivity`。好了第二个 Activity 已经创建好了，创建好了 Activity 后不要忘了需要在 `AndroidManifest.xml` 中注册。由于 Android Studio 已经默认给我们注册了，就不需要了，这个 Activity 也不是主 Activity 也就不需要配置 `<intent-filter>` 了。

下面就是如何启动这第二个 Activity 了，这个时候就需要 `Intent` 这个类了。

**`Intent` 闪亮登场！** `Intent` 是 Android 应用程序中各个组件进行交互的一个重要的方式。**可以通过它指明当前组件想要执行的动作，还可以在不同的组件之间传递数据。** Intent 一般可以用于启动 Activity、Service、发送广播。后面两个我们现在还没有学习到，先看启动 Activity。

**Intent 大致可分为：显式 Intent 和 隐式 Intent** 。先来看显示 Intent 的使用。

Intent 有多个构造函数重载，其中一个是 `Intent(Context context,Class<?> cls)` 。这个方法有两个参数，第一个就是上下文，就是启动 Activity 的上下文，第二个是想要启动的目标 Activity 的 Class。如何使用？Activity 类给我们提供了一个方法`startActivity()` 方法，传入 Intent，就可以启动目标 Activity 了。

```java
   bt.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View view){
                Toast.makeText(MainActivity.this,"显示内容",Toast.LENGTH_SHORT).show();
              // 添加如下代码，启动 SecondActivity
                Intent intent = new Intent(MainActivity.this,SecondActivity.class);
                startActivity(intent);
            }
        });
```

首先传入了 `MainActivity` 这个上下文，传入 `SecondActivity.class` 作为要启动的 Activity。这样 "意图" 就非常明显了。完成了 SecondActivity 的启动。

使用这种方式来启动一个 Activity 的『意图』非常明显了，这就是 **显式 Intent**。

![更多资料](https://upload-images.jianshu.io/upload_images/6737388-494987d373b04b64.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 