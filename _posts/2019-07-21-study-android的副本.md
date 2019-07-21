---
layout: post
title:  "从0系统学Android-1.4日志工具的使用"
date:   2019-07-21 15:14:44
categories:从0系统学Android
tags: Android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}
















![f](https://github.com/sydmobile/sydmobile.github.io/blob/master/pic/%E5%A4%B4%E5%9B%BE%E7%89%87%E4%B8%A2%E5%A4%B1.png?raw=true)

[更多精品文章分类](https://mp.weixin.qq.com/s/B8DP0UMg1fup2_sJVtgjMw)

### 1.4 日志工具

简单介绍一下日志工具，对以后的开发非常有用

#### 1.4.1 使用日志工具 Log

Log 日志工具类提供了 5 个方法来供我们打印信息（级别逐渐提高）

- Log.v()：级别最低，对应 verbose
- Log.d()：打印调试信息，对应 debug
- Log.i()：对应级别 info
- Log.w()：打印警告信息，对应级别 warn
- Log.e()：打印错误信息，级别：error

使用非常简单，一共就五个方法，当然每个方法有不同的重载。

使用：

```java
Log.e("HelloWorldActivity","onCreate");
// 第一个参数是 tag，一般对应类名
// 第二个参数：msg，对应要打印的具体内容
```

这样在 logcat 中可以显示了。

#### 1.4.2 为什么使用 Log 而不用 System.out

对于学习 Java 的我们来说可能在Java 中都是使用 `System.out.println()` 这方法来打印信息的。但是放到 Android 中缺点就太多了：打印时间不可控、不能筛选、没有级别分类。等等

而 Log 配合 LogCat 后就非常的强大了，我们可以筛选出我们需要的信息。

**快捷小提示：**

想要输入 Log.e，只需要输入 loge 然后按下 TAB 键就可以了。

Log 的时候要传入当前类名作为 TAG，那么在方法体的外面输入 logt 然后按下 TAB 就可以自动生成了。

除了这些小的技巧外，logcat 还可以添加过滤器。

![过滤器.png](https://upload-images.jianshu.io/upload_images/6737388-f157859a06eeda0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


`show only....`：表示只显示当前选中的程序

`Firebase` ：Google 提供的一个分析工具，暂时不用管

`No Filters` ：就是没有过滤，会把所有日志打印出来。

当然我们也可以自定义过滤条件。

![自定义过滤器.png](https://upload-images.jianshu.io/upload_images/6737388-092e50727d40c922.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在这里面我们就可以自己定位我们的过滤器了。

看完了过滤器，再来看一级别控制

![级别过滤.png](https://upload-images.jianshu.io/upload_images/6737388-23f8359cbd0bfa22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里主要有五个级别，对应上一节说的五个方法。

选择最低级别 verbose 后，意味着不管我们使用哪个打印方法，都会显示。使用 debug 级别后，只有我们使用 debug 及其以上等级的打印方法，才会显示。依次类推。

最后还有关键字过滤，关键字过滤是支持正则表达式的，这样我们就可以有更加丰富的过滤条件了。

![更多资料](https://upload-images.jianshu.io/upload_images/6737388-494987d373b04b64.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 