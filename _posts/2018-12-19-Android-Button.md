---
layout: post
title:  "Android Button"
date:   2018-12-19 15:14:54
categories: Android
tags: android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}














# Android 点击效果
![new.gif](https://upload-images.jianshu.io/upload_images/6737388-da8f5c478a7e17d2.gif?imageMogr2/auto-orient/strip)

我们平时在开发过程中都可能注意到，我们写的默认的 Button 都是有点击效果的，而且大小也有默认规定的，而 TextView 就没有。就想下面的图片一样。
![GIF.gif](https://upload-images.jianshu.io/upload_images/6737388-9b685c8ef2bf4dd5.gif?imageMogr2/auto-orient/strip)

是有默认效果的。通过查看 Button 的源码我们看到：

![源码说明.jpg](https://upload-images.jianshu.io/upload_images/6737388-f2f69aa2408faf65.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每个 button 有系统默认的风格样式，就是这里的风格样式，使得我们的 button 有了这种效果。下面我们来看看系统默认的 button 风格（注意不同的版本风格可能不同，但大体都是一样的）

![风格.jpg](https://upload-images.jianshu.io/upload_images/6737388-59860f0464f443ed.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过这个构造函数，我们就可以找到 Button 的默认风格了。

![button默认风格.jpg](https://upload-images.jianshu.io/upload_images/6737388-0b1b407c44831be0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就是我们这里使用的默认 Button 的风格（不知道怎么找的看看我前面关于属性的文章），看到这里 Button 的最小高度，最小宽度都有了，这就解释了为什么默认的 Button 就那么大了。当你自己给 Button 设置一个 background 后，你会发现，你的 button 没有默认的那种波浪效果了。那么我们就猜想到肯定和 background 有关。那么我们来看看 button 的默认 background 是如何写的。

![默认背景.jpg](https://upload-images.jianshu.io/upload_images/6737388-b62c6211a1a1ea6d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个就是 background 的默认背景，这里的 `ripple`标签就是点击波浪效果的关键！然后我们仿照自己写一个 background

```html
<ripple xmlns:android="xxxxxxxxxxx"
        android:color="#fffea50b">
    <item android:drawable="@drawable/bcg" />
</ripple>


<inset xmlns:android="http://schemas.android.com/apk/res/android"
       android:insetLeft="4dp"
       android:insetTop="6dp"
       android:insetRight="4dp"
       android:insetBottom="6dp">
    <shape android:shape="rectangle">
        <corners android:radius="@dimen/abc_control_corner_material" />
        <solid android:color="@android:color/holo_blue_dark" />
        <padding android:left="@dimen/abc_button_padding_horizontal_material"
                 android:top="@dimen/abc_button_padding_vertical_material"
                 android:right="@dimen/abc_button_padding_horizontal_material"
                 android:bottom="@dimen/abc_button_padding_vertical_material" />
    </shape>
</inset>        
```

效果图：
![GIF.gif](https://upload-images.jianshu.io/upload_images/6737388-aff21eb796db9260.gif?imageMogr2/auto-orient/strip)

好了，这样我们就实现自定义 background 了。其实关于波浪 `ripple` 的用法还有很多。这里就不再说了，这里只是教大家从源码上分析，借助默认样式，来写出我们的自定义样式。

还有一点，可能会有疑问，那就是 button 下面的阴影效果，其实这里在 5.0 后 Material Design 设计风格。在 Android 5.0 后加入了新的属性 stateListAnimator 使 button 有了阴影效果。具体官方文档：https://developer.android.google.cn/guide/topics/graphics/prop-animation#ViewState 和 https://material.io/design/environment/elevation.html#elevation-shadows-shadows 如果你想去掉这种效果最有效的办法就是 stateListAnimator 的值设置为 @null  当然还有其他办法比如：你可能观察到了上面的 background 的 shape  最外面是 inset ，这样的效果是，如果你设置了 button 的宽 100 高 100 的话，button 的可点击范围是这么大，但是背景是减去 inset 设置的值。这样 button 就有了阴影的空间了。

同样，如果你给你的 TextView 设置了这种风格，那么你的 TextView 就和 button 的样式一样了。好了，现在你就可以完全定义自己的点击效果了！
