---
layout: post
title:  "为什么 AlertDialog 使用Builder 模式呢？"
date:   2019-11-13 16:14:54
categories: Android
tags: Android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}












为什么 AlertDialog 使用Builder 模式呢？

首先说句废话，因为 AlertDialog 太过复杂，内部参数太多，然后不使用构建者模式那么 AlertDialog 的构造方法就可能是： 

[更多精品文章分类](https://mp.weixin.qq.com/s/B8DP0UMg1fup2_sJVtgjMw) 

![声明](https://user-gold-cdn.xitu.io/2019/5/22/16adf0c4dd3185d5?w=1080&h=237&f=png&s=1025845) 

```java
AlertDialog(String title);
AlertDialog(String message)
AlertDialog(int resId)
AlertDialog(int resId, String title, String message);
AlterDialog(int resId, String title, String message, String PositiveButtonString, OnClickListener listener);
AlterDialog(int resId, String title, String message, String PositiveButtonString, OnClickListener listener);
AlterDialog(int resId, String title, String message, String NegativeButtonString, OnClickListener listener);
AlterDialog(int resId, String title, String message, String PositiveButtonString, OnClickListener listener, String NegativeButtonString, OnClickListener listener);
....
```

你觉得这样的代码好吗？假如里面的参数还要多呢？

有的同学就说了，那可以只有一个默认的构造方法，通过这个构造方法生成对象后，然后再调用对象的各种 `set` 方法来调整。这么做的确是达到了最终的效果了。

但是这种做法我举个例子，好比我们都想要得到一个爱读书，身体好，会审美的人。你的这种做法是，先把孩子养大了，其实这个孩子不爱读书，身体不好，审美也不怎么样，然后你再强行的改变孩子。

而使用 Builder 是提前培养孩子，提前告诉孩子要怎么样，最后孩子长大了就是这个样的。

使用 Builder 你可以提前把你想要的属性通过 Builder 的 set 方法设置好，然后再去构建 AlertDialog 对象。

而不是构造出 AlertDialog 对象后再去修改属性。

这就是简单的构建者模式，将一个复杂对象的构建与它的表示分离，使同样的构建过程可以创建不同的表示。