---
layout: post
title:  "一文彻底搞清楚 Material Design"
date:   2019-12-15 15:14:54
categories: Android
tags: Android
excerpt:
mathjax: true
author: sydMobile
---
* content
{:toc}




















### 一文彻底搞清楚 Material Design

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e8ee94a64bcb?w=917&h=663&f=png&s=55430)
![声明](https://user-gold-cdn.xitu.io/2019/5/22/16adf0c4dd3185d5?w=1080&h=237&f=png&s=1025845) 
首先声明以下介绍的关于 `Material Design` 的介绍，**都是基于在 `Android` 环境下**，其实 `Material Design` 是一种为了让 UI 页面更加美观的设计规范，也可以按照这种规范应用到 `iOS`、`Web` 上。

`Material Design` 是 Google 在 2014 年 `I/O` 大会上发布的一种新的设计规范。这种设计风格给 Android  UI 设计带来了很多的变化。让页面变得美感十足。

`Material Design`  是一种综合了传统优秀的设计和科技创新的设计语言。

Material Design 的设计灵感来自现实世界中真正的物质材料。Material Design 设计语言强调根据用户行为凸显核心功能，进而为用户提供操作指引，通过鲜明、形象的颜色差。添加合适的动作来引导用户。

`Material Design` 强调真实性，有立体感。Material Design 的三维体现在光、绘制面和投射阴影。 所有的材料对象都包含 x，y，z 三个维度。z 轴代表了海拔高度，而不是材料的厚度，这一点很多资料都是错误的。**材料的厚度永远是 1 dp 不能改变**。x ，y 就是对应了材料的长宽，可以改变。

这里的材料在`Android` 世界中就是一个个的控件，我们可以把控件想象成现实世界中的物体，规定每个物体的厚度都是固定不变的，永远是 `1dp`，`x,y`就对应了控件的长和宽。

为了体现出真实物体的感觉，引入了光，阴影等一些概念，这些概念我们下面会一一说明。

为了配合这种设计规范，Android 又推出了许多相关的控件。这些控件你既可以单独引用，也可以直接通过`android.design` 包来引入。为了配合 material desig， android 提供了新的主题、新的配合主题的组件、和自定义阴影和新动画 api

来看看 Android 为了配合 `Material Design` 都增加了哪些新的控件：

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e8fb61243bc6?w=408&h=778&f=webp&s=33412)



### 一些基本概念

#### 3D

在真实的物质世界里面，是一个三维的环境。所有的物体都有 x,y,z三个维度。在 Material Design 中，每个物体（也就是你的控件）都有 1 dp 的厚度。

然后这些控件还有海拔的概念，还有影子的概念，这些就体现出了 3 D的感觉。

#### Z 的概念

因为强调现实世界的真实性，引入`Z` 代表了控件的海拔高度。比如说：在一个桌面上，你放了一本书 A，然后在 A 上又放了一本书 B 。这个时候肯定会有层次感，B 相对于桌面的海拔高度和 A 相对于桌面的海拔高度肯定是不一样的。在 Android 中就用 Z 来代表控件的海拔高度。



为了满足 `Material Design` 的层次要求，android 5.0 后增加了 Z 轴，用来表示控件的海拔，海拔的效果具体体现在阴影上。

Z = elevation + translationZ 

View 中的 `Z` 的值有两部分组成：

**注意对 translationZ 的设置，如果单纯的设置控件高度的话，应该是设置 `elevation` 。而不是 `translationZ`**  

- `elevation` ：海拔高度，用来指定控件静止海拔高度  `elevation` 属性 也可以在代码中通过 `setElevation` 来设置。

  在 Android 中 elevation 这个属性代表了海拔高度，这个值是永远有效的，只是如果没有阴影的话，可能体现不出来，只能通过下面的海拔演示来体现出来。

- `TranslationZ`： 动态海拔高度偏移高度，是一个偏移的距离，是用来作动画效果，否则不要使用。

  Translation Z 是动态的，当创建一个项目，增加一个按钮，当按下按钮会阴影变大了。实际上 Elevation 并没有变化，而是 Translation Z 属性在变化。这是 Android 使用默认的状态列表动画，更改 Z 属性。

  按钮的动作效果，默认 FAB 有 6dp 的Elevation，当按下按钮时 translation Z 值开始增加。ViewPropertyAnimator 通过将 translation Z 的值从 0 dp改为 6 dp 来让视图动起来。如果释放按钮，ViewPropertyAnimator 播放动画，将 translationZ 从 6 dp变到 0 dp。我们可以给我们的视图创建自定义状态列表动画，添加到视图上。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e90195b010db?w=1222&h=509&f=png&s=103163)



Z 属性会扩大 View 的显示区域（主要是控件本身大小+阴影），如果它的大小大于或者等于父视图的大小，那么它的阴影效果就无法显示了，view 并不会因为 z 的属性而缩小自身去显示阴影。 

Z属性不仅影响着view的阴影效果，还影响着view的绘制顺序，在同一个父view内部，Z属性越小，绘制的时机就越早。也就是优先被绘制，而z属性越大，则绘制时间越晚，后绘制的将会遮盖住先绘制的，只有Z属性相同，才按照添加的顺序绘制。

#### 海拔

其实上面介绍`Z` 的时候就介绍海拔了，海拔就是为了表现层次感所引入的，现实世界中都有海拔的概念，Z 的值就是代表了海拔。

海拔高度指的是从一个表面到另一个表面之间的距离，元素的海拔高度指明了元素表面之间的距离以及阴影的深度。

海拔高度是两个表面在 Z  轴上的距离，单位也是使用的 dp，一个子元素的海拔是相对于父元素而言的。

海拔高度分为：静止状态海拔高度和动态海拔高度偏移。

静态状态海拔高度：所有的元素都有一个静止的海拔高度（elevation）。

动态海拔高度偏移：指的是从静止状态向目标海拔移动的距离（translationZ）

组件的海拔高度：

- 同一组件在不同的应用中，海拔高度是相同的，比如：不同应用中的浮动操作按钮的海拔是相同的
- 同一组件在不同的平台和设备中，可能会有不同的海拔高度，这主要和环境深度有关。比如：电视具有比桌面更大的深度，因为屏幕更大，用户观看的距离更远。同样电视和桌面的深度比移动设备更深。

某些类型的组件具有响应式的海拔高度，会根据用户的输入（例如 正常状态、获取焦点、按下）和系统事件来改变自身的海拔。这些海拔高度的改变通常是通过动态海拔高度偏移来实现的。

动态海拔高度偏移是组件从静止海拔高度向目标海拔高度所移动的距离。所有组件在被按下时，默认所增加的海拔高度是一样的。一旦输入事件完成或取消，组件会回到原来静止的海拔高度。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e907921f74d9?w=2320&h=960&f=png&s=42169)

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e90b32e4f1c1?w=2320&h=960&f=png&s=268511)

这张图中，控件的海拔高度就不同，表现出层次感。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e912e94c39e9?w=967&h=617&f=png&s=85994)

比如这张图，手机屏幕可以当做是水平面，海拔高度为0，上面有很多控件，它们的海拔高度是不一样的，就表现出层次感了。

##### 海拔的演示

比如 CardView 和 TextView 

```
  <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="200dp">

        <androidx.cardview.widget.CardView
            android:layout_width="match_parent"
            android:layout_height="match_parent">

        </androidx.cardview.widget.CardView>
        <TextView
            
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/app_name"/>
    </RelativeLayout>
这样的话 TextView 是不会显示出来的，因为 TextView 的默认海拔是0 ，就被 Cardview 给挡住了，因为 CardView 的默认海拔是 2dp，如果你将 TextView 的海拔设置为 3dp 这个时候 TextView 就可以显示了。
```

#### 一般控件的标准海拔

- 应用栏：4dp   
- 按钮：静止状态 2dp  按下状态：8dp
- 浮动操作按钮（FAB）静止：6dp  按下：12dp
- 卡片 静止：2dp  浮动状态：8dp
- 菜单和子菜单：菜单：8dp  子菜单：9dp（每个子菜单+1）
- 对话框 24dp
- 抽屉式导航 16dp
- 刷新指示器 3dp
- 快速入口/搜索栏  静止2dp 滚动3dp
- snackbar  6dp
- 开关 1dp

#### 物体的层级结构

所有的物体都是根据父-子关系描述的，子元素会继承父元素的变化属性，比如位置、旋转、海拔高度。同级的物体在层次结构中属于同一层。

比如说：我们桌子上有一层纸，如果我们再贴一张纸，我们的眼睛就会觉得有一个深度。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e917481af0c7?w=1175&h=726&f=png&s=18115)

同样的效果，左边就有深度的感觉，有层次感。

#### 深度（Depth）

深度（depth）的意思就是材质环境中所有的元素都是沿着 Z 轴水平、垂直和以不同的深度移动，在 Z 轴的正方向并且在可是范围内的点的高度。其实就海拔。

#### 轮廓 

默认情况下，所有的view都是矩形的，虽然可以给view设置背景圆形的图片，即可以在界面显示出圆形的内容，但是view的大小实际上依然是矩形，并且设置的图片实际上也是矩形的，只是圆形以外的区域是透明色。

如果根据view大小来生成对应的阴影，就会出现很奇怪的效果，（一个看起来圆形的view展示出的确实一个矩形的阴影）为了解决这个问题，view增加了一个新的描述来指明内容显示的形状，这就是轮廓。

轮廓（Outlines） 代表图形对象的外形状，并确定了对于触摸反馈的波纹区域。

每个 view 都有默认的轮廓（其实有的 View 也没有默认的轮廓，比如 TextView）。如果我们自定义一个 View，其轮廓应该由我们自己来实现其轮廓。

##### 轮廓的实现

①通过shape设置的背景，view会自动根据shape的形状进行轮廓判定，
②通过color设置的背景，view默认其轮廓和view的大小一样。
③但是通过图片进行背景设置，view则无法获知轮廓的形状，这个时候就需要手动进行指定了

##### 手动指定轮廓

当默认轮廓不好使，或者是我们自己定义的View 的话，就需要我们自己通过代码来指定轮廓了。

在代码中，可以通过setOutlineProvider来指定一个view的轮廓。

与轮廓有关的类 `Outline`

Outline是在 android.graphic 下的类，文档说明：

> 定义一个简单的形状，用于作为图形的边界区域
>
> 可以作为一个 View 计算，可以由 Drawable 计算，用来驱动投射出的阴影形状，或者裁剪 View 的内容

总之，这个类就是用来给View指定轮廓的。View 的轮廓还可以通过 `outlineProvider` 属性来设置，默认是 `background` 还有其他值`bounds none paddingBounds`

```java
none:即使你设置了 evaluation 也会显示阴影
background:按背景来显示轮廓，如果 background 是颜色值，则轮廓就是 view 的大小，如果是 shape 则按shape指定的形状来作为轮廓，显示阴影  如果 background 是图片或者透明shape的话只能用代码 `setOutlineProvider()` 来指定轮廓了
bounds:View 的矩形大小作为轮廓
paddingBounds:View 的矩形大小减去 padding 的值后的大小做轮廓  paddedBounds 和bounds类似，不过阴影会稍微向右偏移一点
```

如果我们想创建一个自定义视图，并动态地去改变它的轮廓，这个时候需要使用 `ViewOutlineProvider` 

通过`ViewOutlineProvider`这个类我们可以自己给 View 添加轮廓

```java
public class MyViewOutlineProvider extends ViewOutlineProvider {

    @Override
    public void getOutline(View view, Outline outline) {
        outline.setRoundRect(0, 0, width, height, radius);
    }
}
// 这样这个 View 就有轮廓了，然后通过 setElevation 来修改海拔就可以出现阴影了
//这个方法是提供轮廓，具体的阴影通过 Z 来设置，在轮廓大小固定的情况下，修改 Z  的大小，会占用轮廓的空间，看上去轮廓在变小。
view.setOutProvider(new MyViewOutlienProvider);
// 如果不想让视图有投射阴影，可以设置轮廓提供者为 null
```

##### 裁剪

View 的裁剪是指将 View 按照轮廓裁剪，能改变 View 的形状，如圆形头像：

- 先设置轮廓

- 在设置根据轮廓裁剪 View，目前只支持对矩形、圆形、圆角矩形的裁剪

  `tvClip.setClipToOutline(true)// 设置对 View 进行裁剪`  

  通过 outlin.canClip() 方法来检查是否支持擦肩。

#### 阴影

上面介绍了 3D、海拔、轮廓这些基本的概念，其实这些概念最终有体现效果就是靠阴影。

阴影是一个重要的视觉提示，表示了物体的海拔和运动方向。也是指示两个面之间距离的唯一视觉元素。物体的海拔高度决定了阴影的外观。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e91b79eebf63?w=1214&h=810&f=png&s=25428)

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e91f0bf1d306?w=594&h=754&f=png&s=16028)

阴影还可以用来表示物体的运动方向、表面之间的距离是增加还是减少。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e92351507039?w=1194&h=716&f=png&s=33408)

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e92649d1482e?w=567&h=712&f=png&s=17825)

阴影提供了关于海拔、运动方向和绘制边缘的提示。不同的海拔高度，阴影的表现效果是不同的。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e92a238c8498?w=1174&h=584&f=png&s=18426)

一般来说海拔越高，阴影越大，越低阴影越小，但是海拔太大会出现阴影消失的现象（一般是超过20dp）。当物质材料表面比例改变的时候，其阴影不应该发生改变，海拔发生了变化的时候，其阴影要发生改变。

物质材料内部可以展示任何形状和颜色，但其内容不会增加材料的厚度。 

阴影的产生是不同海拔高度的材料相互叠加产生的，在 Material Design 中，虚拟的光线照射使我的物质材料出现阴影，这里的光有两种光，一种是关键灯，一种是环境灯。关键灯会创建更加锐利的方向性阴影，称为关键阴影。环境光从各个角度出现，创建扩散的柔和阴影，称为环境阴影。

![关键阴影](https://user-gold-cdn.xitu.io/2019/11/15/16e6e92f74ed7d22?w=720&h=720&f=png&s=17544)

![环境阴影](https://user-gold-cdn.xitu.io/2019/11/15/16e6e9347350c4d5?w=720&h=720&f=png&s=13512)

![关键阴影和环境阴影](https://user-gold-cdn.xitu.io/2019/11/15/16e6e93918b291d2?w=720&h=720&f=png&s=17755)

![黑暗下](https://user-gold-cdn.xitu.io/2019/11/15/16e6e93f2c9cff6a?w=720&h=720&f=png&s=14681)



材质环境中的阴影由关键灯光和环境灯光投射共同产生。 在Android和iOS开发中，当光源在沿z轴的各个位置处被“材质”表面阻挡时，会出现阴影。 在Web上，仅通过操纵y轴即可描绘阴影。 以下示例显示了海拔为6dp的卡片。

![](https://user-gold-cdn.xitu.io/2019/11/15/16e6e945696f7019?w=1194&h=520&f=png&s=19660)

##### 阴影的条件

阴影由轮廓和海拔共同决定。

海拔决定了阴影的大小，轮廓决定了阴影的形状。

阴影一定需要有轮廓然后海拔增高后才能被投射出来，两者缺一不可。阴影的底层是 `native` 实现的而不是普通的 2D 渐变效果模拟阴影。

在 Android L  中设置阴影只需两点

1. 设置海拔高度（通过 elevation）
2. 设置轮廓

Button 单纯的施加 `elevation` 是没有阴影效果的，因为 Button 的阴影效果由 `stateListAnimatior` 来决定了，如果想要自己给 `Button` 添加的 `elevation` 有效果的话，必须将 `stateListAnimator = "@null"`  才可以。但是如果`stateListAnimator`设置为 null 后，点击的海拔高度动画就没有了，为此你可以在 Button 外套一层 `LinearLayout`给 `LinearLayout 设置 elevation`，记住`LinearLayout`一定要有背景 。但是设置最好不需要这样，用 Button 自身的阴影效果就可以了，它的阴影会根据 Button 在页面中的位置的不同阴影还不同。 详见 [Button](<https://blog.csdn.net/sydMobile/article/details/79642786>)



参考[Materila Design中文](<https://www.mdui.org/design/material-design/introduction.html>)  [Materila Design官网](<https://material.io/design/environment/elevation.html#default-elevations>)

[彻底理解Android中的阴影](<https://segmentfault.com/a/1190000013386151>) 

[各种阴影](<https://segmentfault.com/a/1190000011809297>) 

[中文官网Material动画效果](<http://hukai.me/android-training-course-in-chinese/material/animations.html>)
![](https://user-gold-cdn.xitu.io/2019/10/10/16db5064ec1e7b6d?w=1240&h=620&f=jpeg&s=145465)