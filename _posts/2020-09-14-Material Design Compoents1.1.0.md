---
layout: post
title:  "Material Design Compoents1.1.0"
date:   2020-09-14 22:14:54
categories: MDC
tags: MDC
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}
















### Material Design Compoents  1.1.0

![](http://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/61d4af5f0088411d83a0e5f966e7ec64~tplv-k3u1fbpfcp-zoom-1.image) 

增加了 Material Theming，新的组件、黑暗主题支持、等等

新的功能：

- 所有组件都支持黑暗主题

- 新的日期选择器（具有范围选择功能和提升可访问功能）
- 扩展 Floating Action 按钮
- 切换按钮组
- 支持 Android 10 进行边缘手势导航的组件
- 改善无障碍功能
- 全新的 Material Theming (形状、板式、颜色)
- 稳定性和质量改进

### MDC的背景说明

Material Components for Android（MDC）是从以前的 `Design Support Library` 库演变而来，是与 `AndroidX` 来搭配的。考虑到版本兼容和过渡，一开始的 `1.0.0` 版本与 `design 库 28.0.0` 是等价的。命名发生了改变从 `com.android.support.design`到 `com.google.android.material` 。不过后续更新 `design` 库就不再更新了，也就是说 `design` 库就永远的停留在了 `1.0.0` 这个版本了。

从 `1.0.0` 开始 `Material Design` 的规范不断发展。出现了新的规范、准则和新的组件，来更好的代表品牌同时保持了 `Material` 的核心原则。`MDC` 的目的是为开发者提供一个库，该库通过代码形式来实现这些组件和准则。随着指南不断的变化更新，MDC 将进行调整并更新来满足新的规范。

### 1.1.0 有什么新功能

**MDC从 `1.0.0` 开始发生了大量的改变！**如果你还在使用测试版或者`1.0.0` 请尽快迁移到 `1.1.0` 版本或者更新版本。

#### Material Theming

`Material Theming` 可以让你更好的自定义 `Material Design` 来体现我们的品牌、颜色、字体和形状的选择。

`MDC 1.1.0` 在您的 Android 应用中启用 `Material Theming` 。所有组件都支持通过主题、样式、新属性和自定义类（比如：`MaterialShapeDrawable`） 来调整其颜色、字体和形状。

```xml
// 这个地方要继承 Theme.MaterialComponents.*
<style name="Theme.MyApp" parent="Theme.MaterialComponents.*">    <!-- Color attributes -->
    <item name="colorPrimary">@color/green_500</item>
    <item name="colorPrimaryVariant">@color/green_700</item>
    <item name="colorOnPrimary">@color/black</item>
    <item name="colorSecondary">@color/orange_500</item>
    <item name="colorSecondaryVariant">@color/orange_300</item>
    <item name="colorOnSecondary">@color/black</item>
    <item name="colorError">@color/red_700</item>
    <item name="colorOnError">@color/white</item>
    <item name="colorSurface">@color/white</item>
    <item name="colorOnSurface">@color/black</item>
    <item name="android:colorBackground">@color/white</item>
    <item name="colorOnBackground">@color/black</item>    <!-- Type attributes -->
    <item name="textAppearanceHeadline1">
        @style/TextAppearance.MyApp.Headline1
    </item>
    <item name="textAppearanceHeadline2">
        @style/TextAppearance.MyApp.Headline2
    </item>
    <item name="textAppearanceHeadline3">
        @style/TextAppearance.MyApp.Headline3
    </item>
    <item name="textAppearanceHeadline4">
        @style/TextAppearance.MyApp.Headline4
    </item>
    <item name="textAppearanceHeadline5">
        @style/TextAppearance.MyApp.Headline5
    </item>
    <item name="textAppearanceHeadline6">
        @style/TextAppearance.MyApp.Headline6
    </item>
    <item name="textAppearanceSubtitle1">
        @style/TextAppearance.MyApp.Subtitle1
    </item>
    <item name="textAppearanceSubtitle2">
        @style/TextAppearance.MyApp.Subtitle2
    </item>
    <item name="textAppearanceBody1">
        @style/TextAppearance.MyApp.Body1
    </item>
    <item name="textAppearanceBody2">
        @style/TextAppearance.MyApp.Body2
    </item>
    <item name="textAppearanceCaption">
        @style/TextAppearance.MyApp.Caption
    </item>
    <item name="textAppearanceButton">
        @style/TextAppearance.MyApp.Button
    </item>
    <item name="textAppearanceOverline">
        @style/TextAppearance.MyApp.Overline
    </item>    <!-- Shape attributes -->
    <item name="shapeAppearanceSmallComponent">
        @style/ShapeAppearance.MyApp.SmallComponent
    </item>
    <item name="shapeAppearanceMediumComponent">
        @style/ShapeAppearance.MyApp.MediumComponent
    </item>
    <item name="shapeAppearanceLargeComponent">
        @style/ShapeAppearance.MyApp.LargeComponent
    </item>
    
</style>
```

#### 新组件

`Material Components` 库中有很多新的组件添加到了 `MDC 1.1.0` 中。并且已经存在的组件也是通过最新的设计有了新的 `style` 如果您使用的是`Design库`或者 `MDC 1.0.0` 那么组件将自动采用这些新样式。例如，文字有新的默认的 `appearance`

MDC `1.1.0` 中提供的一些新组件和更新组件包括：

- 扩展 FAB
- 日期选择器
- 切换按钮
- 底部应用栏

#### 黑色主题支持

在 Android 10 中引入了系统范围的深色主题支持。连同 Material Design 指南。MDC 可以立即使用 `Material Dark` 主题。它以现有的 `AppCompat DayNight`功能为基础，因此不用从头开始实现它：

- 主题：现在所有的 MDC 主题都会有不同的 `DayNight` 形式。这些会根据设备配置自动在 `-night` 和 `-not-night` 资源定位符之间切换。
- 新颜色：默认调色板已扩展为了深色主题已经扩展了。应该进行调整 `colorPrimary`  `colorSecondary` 以使品牌在黑暗主题中的饱和度降低。默认情况下 `colorSurface android:colorBackground` 使用深灰而不是黑色来减轻眼睛疲劳，使高程度更明显，并确保与文本和其他元素形成适当的对比度。
- 海拔表面增亮：所有 MDC 组件都支持其表面增亮来传达黑暗主题中的海拔。指南中的白色覆盖投影映射到组件上设置的 `elevation` 的数值。
- 可访问性：MDC 利用颜色来区分是否可以访问。（colorSurface 和 colorOnSurface）在深色主题中区分可访问和不可访问一个重要的方面是通过颜色之间有足够的对比度！MDC 现在使用推荐的颜色和不透明度来确保是这种情况。
- Primary 和 Surface 颜色切换：MDC 组件遵循指南，减少在深色主题中使用 Primary 色。例如：可以在工具栏中看到使用 `colorSurface` 来替换了 `colorPrimary` 作为其背景色。这是由一个新的颜色属性 `colorPrimarySurface` （更加当前的模式在 `colorPrimary` 和 `colorSurface`之间切换）和组件的`PrimarySurface` style 来提供支持。

#### Android 10 手势支持

手势导航是在 Android10 中引入的。某些 MDC 组件常常处于主手势的区域（比如，`BottomNavigationView` 以及从底部向上滑动的原始手势）。相关组件已经更新，以考虑这些手势区域以及设备方向。适当的 `padding/margin` 值会自动被申请，用 `WindowInsets` API(在 Android 10 或者更高版本)。



#### Accessibility improvements

许多可 `accessibility` 改进已经加入到 MDC 组件中。这主要包含更好的 "话语提示" 在有用的内容描述、功能和各部分的排序。例如，`TextInputLayout`现在按正确的顺序读取其提示，输入以及帮助程序或错误文本。

### MDC的下一步计划

我们已经收到了您关于 MDC 版本的反馈。我们致力于更新并且整合您的重要贡献。

原文地址：https://medium.com/google-design/material-design-components-for-android-1-1-0-are-now-available-45e1d576037c 


![](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad04816bc26b4e248f6778abad7e5c4c~tplv-k3u1fbpfcp-zoom-1.image)

