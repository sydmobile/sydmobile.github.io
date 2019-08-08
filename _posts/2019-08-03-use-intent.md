---
layout: post
title:  "从0系统学Android-2.4隐式Intent"
date:   2019-08-03 15:14:54
categories: 从0系统学Android
tags: Android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}














![f](https://github.com/sydmobile/sydmobile.github.io/blob/master/pic/%E5%A4%B4%E5%9B%BE%E7%89%87%E4%B8%A2%E5%A4%B1.png?raw=true)

**本系列文章，参考《第一行代码》，作为个人笔记**

更多内容：[更多精品文章分类](https://mp.weixin.qq.com/s/B8DP0UMg1fup2_sJVtgjMw)

####  使用隐式 Intent

相对于显示 Intent ，隐式 Intent 比较含蓄。这种方式不明确指出我们想要启动哪一个 Activity。而是定义了一系列更为抽象的 `action` 和 `category` 等信息。然后交给系统去分析这个 Intent ，并帮我们找出这个合适Activity。

合适的 Activity 就是指的可以响应这个隐式 Intent 的 Activity。	

通过在 `<activity>` 标签下配置 `<intent-filter>` 的内容，可以指定当前 Activity 能够响应的 action 和 category。在 `AndroidManifest.xml` 中添加：

```java
<activity android:name = ".SecondActivity">
	<intent-filter>
		<action android:name ="com.syd.start"/>
		<category android:name ="android.intent.category.DEFAULT"/>
  </intent-filter>
</activity>
```

在 `<action>` 标签中我们指明了当前 Activity 可以响应 `com.example.activitytest.ACTION_START` 这个 action。`<category>` 标签包含了一些附加信息，更加精确的指明了当前 Activity 能够响应的 Intent 中还可能带有的`category` 只是可能带有，如果 Intent 中带有 `category` 则要启动的 Activity 的注册中必须有这个 `category`才可以。如果 Intent 中没有带有 `category` 也是可以的。不过 Activity 在声明的时候只要声明了 `action` 就要带一个 `<category android:name = "android.intent.category.DEFAULT"` 否则使用 action 启动的时候会报错，这是因为用 `startActivity()`方法的时候会自动将这个 `category` 添加到 Intent 中去。

在 `MainActivity` 中将显示启动该为隐式启动

```java
bt.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View view){
                Toast.makeText(MainActivity.this,"显示内容",Toast.LENGTH_SHORT).show();
                Intent intent = new Intent("com.syd.start");
                startActivity(intent);
            }
        });
```

这里使用了 `Intent` 的另外一个构造函数直接将 `action` 的字符传了过去，表明我们想要启动的 Activity 需要能够响应 `com.syd.start` 这个 Action。前面我们已经在 `AndroidManifest.xml` 中表明了 `SecondActivity` 可以响应这种 Action 了。

这个时候重新运行程序，点击按钮，就可以使用隐式 Intent 来启动 `SecondActivity` 了。

**每个 Intent 中只能指定一个 action，但是可以指定多个 category**