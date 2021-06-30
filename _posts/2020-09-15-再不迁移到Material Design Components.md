---
layout: post
title:  "再不迁移到Material Design Components"
date:   2020-09-15 22:14:54
categories: MDC
tags: MDC
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}
















翻译自国外文档加自己理解 [原文](https://medium.com/androiddevelopers/migrating-to-material-components-for-android-ec6757795351)

我们最近宣布了 `Material Design Components（MDC）1.1.0` ，这是一个库更新，为您的 Android 应用程序带来了 `Material Theming` 、新的组件、深色主题和其他令人兴奋的功能。

MDC取代了设计支持库。本指南将向您展示如何迁移代码库，以便您可以使用新的属性，样式和小部件。

### 精简的主题示例

本指南使用了精简的应用程序来演示迁移过程。它使用AppCompat主题，设计支持库中的小部件（包括具有自定义背景的按钮）以及需要迁移的各种其他元素。我们将从使用传统AppCompat模板的应用程序主题开始：

```xml
<style name="Theme.App" parent="Theme.AppCompat.*">
    <item name="colorPrimary">@color/navy_700</item>
    <item name="colorPrimaryDark">@color/navy_900</item>
    <item name="colorAccent">@color/green_300</item>
</style>
```

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4093765d2726433e969cb511af4d243f~tplv-k3u1fbpfcp-zoom-1.image)

使用 `AppCompat` 和 `Design Support Library` 的 APP

### 从 `Support Library` 迁移到 `JetPack` 

在使用MDC之前，您需要从支持库迁移到[Android Jetpack](https://developer.android.com/jetpack/)。Jetpack使用新的`androidx.*`名称空间，并将以前的支持库程序包拆分为单独维护的语义版本化的库，从而提供部分功能的新库。MDC是使用AndroidX库构建的，因此必须进行迁移。

要迁移到 AndroidX ，建议您遵循官方开发人员文档。 Android Studio中的 重构 > 迁移到 AndroidX 工具会将您的 `Design Support Library` 依赖重构成 `MDC`。



### 更新到 MDC

首先要将`build.gradle` 依赖中

`com.android.support:design:28.0.0`  修改成 `com.google.android.material:material:1.0.0`

### 更改主题

需要将 app 的主题修改成 `Material Components` 主题的子类

`<style name = "Theme.App" parent = "Theme.AppCompat.*"`  修改成

`<style name = "Theme.App" parent = "Theme.MaterialComponents.">`

在 `MDC` 主题中有样式和 `AppCompat` 一一对应，在大多数情况下，只需要简单的将 `AppCompat` 替换成 `MaterialComponents` 就可以了

| AppCompat theme                            | MDC-Android theme                                   |
| ------------------------------------------ | --------------------------------------------------- |
| `Theme.AppCompat`                          | `Theme.MaterialComponents`                          |
| `Theme.AppCompat.NoActionBar`              | `Theme.MaterialComponents.NoActionBar`              |
| `Theme.AppCompat.Dialog.*`                 | `Theme.MaterialComponents.Dialog.*`                 |
| `Theme.AppCompat.DialogWhenLarge`          | `Theme.MaterialComponents.DialogWhenLarge`          |
| `Theme.AppCompat.Light`                    | `Theme.MaterialComponents.Light`                    |
| `Theme.AppCompat.Light.DarkActionBar`      | `Theme.MaterialComponents.Light.DarkActionBar`      |
| `Theme.AppCompat.Light.NoActionBar`        | `Theme.MaterialComponents.Light.NoActionBar`        |
| `Theme.AppCompat.Light.Dialog.*`           | `Theme.MaterialComponents.Light.Dialog.*`           |
| `Theme.AppCompat.Light.DialogWhenLarge`    | `Theme.MaterialComponents.Light.DialogWhenLarge`    |
| `Theme.AppCompat.DayNight`                 | `Theme.MaterialComponents.DayNight`                 |
| `Theme.AppCompat.DayNight.DarkActionBar`   | `Theme.MaterialComponents.DayNight.DarkActionBar`   |
| `Theme.AppCompat.DayNight.NoActionBar`     | `Theme.MaterialComponents.DayNight.NoActionBar`     |
| `Theme.AppCompat.DayNight.Dialog.*`        | `Theme.MaterialComponents.DayNight.Dialog.*`        |
| `Theme.AppCompat.DayNight.DialogWhenLarge` | `Theme.MaterialComponents.DayNight.DialogWhenLarge` |

| AppCompat theme overlay              | MDC-Android theme overlay                               |
| ------------------------------------ | ------------------------------------------------------- |
| `ThemeOverlay.AppCompat`             | `ThemeOverlay.MaterialComponents`                       |
| `ThemeOverlay.AppCompat.Light`       | `ThemeOverlay.MaterialComponents.Light`                 |
| `ThemeOverlay.AppCompat.Dark`        | `ThemeOverlay.MaterialComponents.Dark`                  |
| `ThemeOverlay.AppCompat.*.ActionBar` | `ThemeOverlay.MaterialComponents.*.ActionBar.*`         |
| `ThemeOverlay.AppCompat.Dialog.*`    | `ThemeOverlay.MaterialComponents.Dialog.*`              |
| N/A                                  | `ThemeOverlay.MaterialComponents.*.BottomSheetDialog`   |
| N/A                                  | `ThemeOverlay.MaterialComponents.MaterialAlertDialog.*` |
| N/A                                  | `ThemeOverlay.MaterialComponents.MaterialCalendar.*`    |
| N/A                                  | `ThemeOverlay.MaterialComponents.Toolbar.*`             |

### 例子更新

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f034079b6c084537b0815b1fadbcfe8a~tplv-k3u1fbpfcp-zoom-1.image)

### Button 改变

从 `Design` 库到 `MDC` ，样式变成 `Theme.MaterialComponents.*` 后有了一些变化。拿 Button 来举例，Button失去了自定义背景。现在 Button 有了一个绿色的强调色并且字体间的间距变大了。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b99077efc5c4da89f22b7047bcc75dc~tplv-k3u1fbpfcp-zoom-1.image)

那么为什么会这样呢？我们先来看一下布局

```xml
<Button
    android:id="@+id/containedButton"
    // 这是自定义的某种颜色的背景
    android:background="@drawable/bg_button_gradient"
    android:textColor="@android:color/white"
    ... />
<Button
    android:id="@+id/textButton"
    style=”?attr/borderlessButtonStyle”
    ... />
```

之所以出现这种情况是因为，在填充布局的时候，会自动将我们布局中的普通控件替换成 `MDC` 控件。

和 AppCompat 一样，MDC 会在填充的时候用 MDC 等效的控件来替换某些原始控件。这样就可以发布新功能和错误修正了，而不必将所有声明都换成新的类型。这是通过 `MaterialComponentsViewInflater` 来完成的，它属于 `AppCompatViewInflater`  的子类。

映射关系：

| Framework widget       | AppCompat widget (replaced by [`AppCompatViewInflater`](https://developer.android.com/reference/androidx/appcompat/app/AppCompatViewInflater)) | MDC-Android widget (replaced by [`MaterialComponentsViewInflater`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/theme/MaterialComponentsViewInflater.java)) |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Button`               | [`AppCompatButton`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatButton) | [`MaterialButton`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/button/MaterialButton.java) |
| `CheckBox`             | [`AppCompatCheckBox`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatCheckBox) | [`MaterialCheckBox`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/checkbox/MaterialCheckBox.java) |
| `RadioButton`          | [`AppCompatRadioButton`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatRadioButton) | [`MaterialRadioButton`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/radiobutton/MaterialRadioButton.java) |
| `TextView`             | [`AppCompatTextView`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatTextView) | [`MaterialTextView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textview/MaterialTextView.java) |
| `AutoCompleteTextView` | [`AppCompatAutoCompleteTextView`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatAutoCompleteTextView) | [`MaterialAutoCompleteTextView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textfield/MaterialAutoCompleteTextView.java) |

**注意：** 在 `MDC 1.0.0` 中只有 `Button` 控件被替换了。

我们例子中如果是 `Theme.AppCompat.*` 的主题，那么就会把 `Button` 用 `AppCompatButton` 来替换。现在把主题修改成 `Theme.MaterialComponents.*` ，那么就会把 `Button` 替换成 `MaterialButton` ，会有默认的 style

和 `AppCompatButton` 不同的是 `MaterialButton` 不支持自定义背景。到 `1.2.0-alpha06` 版本开始支持。使用 `Shape` 可以进行变通。下面章节会详细介绍。

### 更新到 MDC 1.1.0

从 1.0.0 到 1.1.0 有了很多新变化：

- 完整的 `Material Theming` 
- `Dark Theme` 支持
- Android 10 手势导航支持
- 新组件：扩展 FAB、date picker、badges、toggle buttons
- 无障碍功能提升、bug 修复等等

`implementation ‘com.google.android.material:material:1.1.0’`



### 一些出乎意料的改变和普通问题

MDC `1.1.0`更改了一些默认的小部件样式，以更好地符合“材料设计”准则。但是，升级后，您可能会注意到某些控件颜色和其他属性的某些意外更改。


![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82c0b14243d147e99c0b6cc3b5012ed8~tplv-k3u1fbpfcp-zoom-1.image)

在上面的示例中，按钮发生了变化、文本和图标的颜色发生了变化。FAB 现在变成了蓝绿色，并且文本字段看起来完全不同。不用担心。我们的当前主题中可能是丢失了一些重要的 MDC 属性，同时有一些重要的 AppCompat 或者原有属性（android：xxx）不再需要。下面我们通过一些常见的迁移方案来了解一下这些问题

#### 文字栏位改变

在 MDC 中，文字字段默认样式发生了改变。改进版本是经过用户调查研究的。


![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3928214b08304946b73ff5e569890346~tplv-k3u1fbpfcp-zoom-1.image)

我们建议您使用这个版本，来提高可用性和可配置项性。但是我们意识到这可能并不适合您的品牌和设计系统。

要恢复为旧的文本字段可以在布局中添加样式

```xml
<com.google.android.material.textfield.TextInputLayout
    ...
+    style="@style/Widget.Design.TextInputLayout">
    ...
</com.google.android.material.textfield.TextInputLayout>
```

或者你也可以在主题中给所有的文本设置默认样式

```xml
<style name="Theme.App" parent="Theme.MaterialComponents.*">
    ...
+    <item name=”textInputStyle”>@style/Widget.App.TextInputLayout</item>
</style>

+<style name=”Widget.App.TextInputLayout” parent=”Widget.Design.TextInputLayout”>
+    <!-- Custom attrs -->
+</style>
```

![](F:\MyFile\mybook\文章使用图片\MaterialDesign\支持库Text.png)
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f25c1ef3c128453188c75a7a4694ad2b~tplv-k3u1fbpfcp-zoom-1.image)

#### 更喜爱 MDC 样式和控件

如上所述，先前支持库的风格已经变成了 MDC 的一部分。在大多数的情况下，我们都可以通过 `Widget.MaterialComponents.*` 来替换 `Widget.Design.*` 样式。并且还启用了新的属性，虽然可以不使用，但是我们建议还是采用新的 MDC 样式！

建议使用 `MDC` 组件来替换`AppCompat` 或者 `MaterialButton` （如果有的话）这些组件默认情况下使用更新后的材料设计指南。并且支持启用 `Material Theming` 和其他功能。

下面这几种情况应该考虑

- 在布局中写的控件如果有对应的 MDC 控件的话，直接使用 MDC 控件
- 任何的风格，默认风格和默认风格属性应该改变成 MDC 版本
- 在编程中或者自定义类的父级类使用的任何控件都应该为 MDC 版本。

完整 控件和样式映射表

| MDC-Android widget (moved from Design Support Library)       | Design Support Library default style   | MDC-Android default style                                    | Default style attr          |
| ------------------------------------------------------------ | -------------------------------------- | ------------------------------------------------------------ | --------------------------- |
| [`AppBarLayout`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/appbar/AppBarLayout.java) | `Widget.Design.AppBarLayout`           | `Widget.MaterialComponents.AppBarLayout.*`                   | `appBarLayoutStyle`         |
| [`BottomNavigationView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/bottomnavigation/BottomNavigationView.java) | `Widget.Design.BottomNavigationView`   | `Widget.MaterialComponents.BottomNavigationView`             | `bottomNavigationStyle`     |
| [`BottomSheetBehavior`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/bottomsheet/BottomSheetBehavior.java) | `Widget.Design.BottomSheet.Modal`      | `Widget.MaterialComponents.BottomSheet.*`                    | `bottomSheetStyle`          |
| [`BottomSheetDialog`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/bottomsheet/BottomSheetDialog.java) [`BottomSheetDialogFragment`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/bottomsheet/BottomSheetDialogFragment.java) | `Theme.Design.Light.BottomSheetDialog` | `Theme.MaterialComponents.*.BottomSheetDialog` `ThemeOverlay.MaterialComponents.*.BottomSheetDialog` | `bottomSheetDialogTheme`    |
| [`CollapsingToolbarLayout`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/appbar/CollapsingToolbarLayout.java) | `Widget.Design.CollapsingToolbar`      | N/A                                                          | N/A                         |
| [`FloatingActionButton`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/floatingactionbutton/FloatingActionButton.java) | `Widget.Design.FloatingActionButton`   | `Widget.MaterialComponents.FloatingActionButton`             | `floatingActionButtonStyle` |
| [`NavigationView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/navigation/NavigationView.java) | `Widget.Design.NavigationView`         | `Widget.MaterialComponents.NavigationView`                   | `navigationViewStyle`       |
| [`Snackbar`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/snackbar/Snackbar.java) | `Widget.Design.Snackbar`               | `Widget.MaterialComponents.Snackbar`                         | `snackbarStyle`             |
| [`TabLayout`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/tabs/TabLayout.java) [`TabItem`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/tabs/TabItem.java) | `Widget.Design.TabLayout`              | `Widget.MaterialComponents.TabLayout`                        | `tabStyle`                  |
| [`TextInputLayout`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textfield/TextInputLayout.java) [`TextInputEditText`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textfield/TextInputEditText.java) | `Widget.Design.TextInputLayout`        | `Widget.MaterialComponents.TextInputLayout.*`                | `textInputStyle`            |



| AppCompat widget                                             | AppCompat default style                                      | AppCompat default style attr          | MDC-Android widget                                           | MDC-Android default style                                    | MDC-Android default style attr                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| [`AlertDialog.Builder`](https://developer.android.com/reference/androidx/appcompat/app/AlertDialog.Builder) | `AlertDialog.AppCompat` `ThemeOverlay.AppCompat.Dialog.Alert` | `alertDialogStyle` `alertDialogTheme` | [`MaterialAlertDialogBuilder`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/dialog/MaterialAlertDialogBuilder.java) | `MaterialAlertDialog.MaterialComponents` `ThemeOverlay.MaterialComponents.MaterialAlertDialog` | `alertDialogStyle` `materialAlertDialogTheme` |
| [`AppCompatAutoCompleteTextView`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatAutoCompleteTextView) | `Widget.AppCompat.AutoCompleteTextView`                      | `autoCompleteTextViewStyle`           | [`MaterialAutoCompleteTextView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textfield/MaterialAutoCompleteTextView.java) | `Widget.MaterialComponents.AutoCompleteTextView.*` `ThemeOverlay.MaterialComponents.AutoCompleteTextView.*` | `autoCompleteTextViewStyle`                   |
| [`AppCompatButton`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatButton) | `Widget.AppCompat.Button`                                    | `buttonStyle`                         | [`MaterialButton`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/button/MaterialButton.java) | `Widget.MaterialComponents.Button`                           | `materialButtonStyle`                         |
| [`AppCompatCheckBox`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatCheckBox) | `Widget.AppCompat.CompoundButton.CheckBox`                   | `checkboxStyle`                       | [`MaterialCheckbox`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/checkbox/MaterialCheckBox.java) | `Widget.MaterialComponents.CompoundButton.CheckBox`          | `checkboxStyle`                               |
| [`AppCompatImageView`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatImageView) | N/A                                                          | N/A                                   | [`ShapeableImageView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/imageview/ShapeableImageView.java) | `Widget.MaterialComponents.ShapeableImageView`               | N/A                                           |
| [`AppCompatRadioButton`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatRadioButton) | `Widget.AppCompat.CompoundButton.RadioButton`                | `radioButtonStyle`                    | [`MaterialRadioButton`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/radiobutton/MaterialRadioButton.java) | `Widget.MaterialComponents.CompoundButton.RadioButton`       | `radioButtonStyle`                            |
| [`AppCompatTextView`](https://developer.android.com/reference/androidx/appcompat/widget/AppCompatTextView) | `Widget.AppCompat.TextView`                                  | N/A                                   | [`MaterialTextView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/textview/MaterialTextView.java) | `Widget.MaterialComponents.TextView`                         | N/A                                           |
| [`CardView`](https://developer.android.com/reference/androidx/cardview/widget/CardView) | `CardView`                                                   | `cardViewStyle`                       | [`MaterialCardView`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/card/MaterialCardView.java) | `Widget.MaterialComponents.CardView`                         | `materialCardViewStyle`                       |
| [`PopupMenu`](https://developer.android.com/reference/androidx/appcompat/widget/PopupMenu) | `Widget.AppCompat.PopupMenu.*`                               | `popupMenuStyle`                      | N/A                                                          | `Widget.MaterialComponents.PopupMenu.*`                      | `popupMenuStyle`                              |
| [`SwitchCompat`](https://developer.android.com/reference/androidx/appcompat/widget/SwitchCompat) | `Widget.AppCompat.CompoundButton.Switch`                     | `switchStyle`                         | [`SwitchMaterial`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/switchmaterial/SwitchMaterial.java) | `Widget.MaterialComponents.CompoundButton.Switch`            | `switchStyle`                                 |
| [`Toolbar`](https://developer.android.com/reference/androidx/appcompat/widget/Toolbar) | `Widget.AppCompat.Toolbar`                                   | `toolbarStyle`                        | [`MaterialToolbar`](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/appbar/MaterialToolbar.java) | `Widget.MaterialComponents.Toolbar`                          | `toolbarStyle`                                |

最新的组件完整列表以及使用文档：https://material.io/develop/android/

### 示例更新

用 MDC 版本的组件来替换

```xml
<!-- Copyright 2020 Google LLC.
   SPDX-License-Identifier: Apache-2.0 -->
-<androidx.cardview.widget.CardView
+<com.google.android.material.card.MaterialCardView
    android:id="@+id/card"
    ...>
    ...
-</androidx.cardview.widget.CardView>
+</com.google.android.material.card.MaterialCardView>

-<androidx.appcompat.widget.SwitchCompat
+<com.google.android.material.switch.SwitchMaterial
    android:id="@+id/switch"
    ... />
```

### 颜色

MDC的颜色调色板直接从 `Material Design color system` 中绘制。

由于MDC-Android，AppCompat和框架之间共享历史记录，因此，颜色属性集包括以下内容：

- 框架中已适当命名的现有属性（例如`android:colorBackground`）
- AppCompat中已适当命名的现有属性（例如`colorPrimary`和`colorError`）
- 新的属性由MDC介绍（如`colorSurface`，`colorOnPrimary`等）

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c0c86632b663463cabc0aabdbd99d862~tplv-k3u1fbpfcp-zoom-1.image)

MDC窗口小部件使用这些属性来为其背景，文本，图标等着色。要了解哪些小部件使用哪种颜色，需要检查[源代码中](https://github.com/material-components/material-components-android)的默认小部件样式。

AppCompat和框架中还存在一些颜色，但不再适用于此新系统。该`Theme.MaterialComponents.*`主题尽最大努力向后兼容他们，例如小部件，这些旧属性。

`<item name="colorAccent">?attr/colorSecondary</item>`

但是，您应该考虑不推荐使用这些属性。使用更合适的MDC属性或逐步淘汰它们。

请参阅下面的颜色属性映射表：

**注意 AppCompat 中的颜色属性就不要再使用了**

| AppCompat /框架颜色属性                                      | MDC-Android颜色属性                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `colorPrimary`                                               | `colorPrimary`                                               |
| `colorPrimaryDark`                                           | `colorPrimaryVariant` （`android:statusBarColor`明确指定）   |
| 不适用                                                       | `colorOnPrimary`                                             |
| `colorAccent`                                                | `colorSecondary`                                             |
| 不适用                                                       | `colorSecondaryVariant`                                      |
| 不适用                                                       | `colorOnSecondary`                                           |
| 不适用                                                       | `colorSurface`                                               |
| 不适用                                                       | `colorOnSurface`                                             |
| `android:colorBackground`                                    | `android:colorBackground`                                    |
| 不适用                                                       | `colorOnBackground`                                          |
| `colorError`                                                 | `colorError`                                                 |
| 不适用                                                       | `colorOnError`                                               |
| `android:textColorPrimary`，`android:textColorSecondary`等等。 | N / A （建议将MDC设置为“ on”属性或使用默认值）               |
| `colorControlNormal`，`colorControlHighlight`，`colorControlActivated` | N / A （“接通”或ATTRS使用默认偏好MDC） ***注意：**这些仍然被用来着色[`MaterialCheckBox`](https://github.com/material-components/material-components-android/tree/master/lib/java/com/google/android/material/checkbox/MaterialCheckBox.java#L119)，[`MaterialRadioButton`](https://github.com/material-components/material-components-android/tree/master/lib/java/com/google/android/material/radiobutton/MaterialRadioButton.java#L110)，[`SwitchMaterial`](https://github.com/material-components/material-components-android/tree/master/lib/java/com/google/android/material/switchmaterial/SwitchMaterial.java#L148)和[`MaterialToolbar`图标](https://github.com/material-components/material-components-android/tree/master/lib/java/com/google/android/material/appbar/res/values/styles.xml#L78)。* |

#### 例子

```xml
<style name="Theme.App" parent="Theme.MaterialComponents.*">
    ...
    <item name="colorPrimary">@color/navy_700</item>
-    <item name="colorPrimaryDark">@color/navy_900</item>
+    <item name="colorPrimaryVariant">@color/navy_900</item>
-    <item name="colorAccent">@color/green_300</item>
+    <item name="colorSecondary">@color/green_300</item>
+    <item name=”colorSecondaryVariant”>@color/green_500</item>
+    <item name="android:statusBarColor">@color/navy_900</item>
</style>
```

`@color`对于包含的按钮文本颜色，我们还应该使用新的“ on”颜色属性

```xml
<!-- Copyright 2020 Google LLC.
   SPDX-License-Identifier: Apache-2.0 -->
<Button
-    android:textColor="@android:color/white"
+    android:textColor="?attr/colorOnPrimary"
    ... />
```

### 字体板式

新的 `TextAppearance` 样式/属性

MDC字体板式直接从[Material Design类型系统中](https://material.io/design/typography/)提取。表达的意思就是紧贴 Material Design 风格

引入了一组新的`TextAppearance.MaterialComponents.*`样式和相应的`textAppearance*`主题属性，它们替代了现有的AppCompat /框架样式。

![](F:\MyFile\mybook\文章使用图片\MaterialDesign\textappearance.png)
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d737050018f94b3ab1aafa9b64e81e67~tplv-k3u1fbpfcp-zoom-1.image)

MDC小部件使用这些属性来设置文本样式。要知道哪些窗口小部件使用哪种类型板式，需要检查[源代码中](https://github.com/material-components/material-components-android)的默认窗口小部件样式。

请参阅下面的完整类型样式和属性映射表： 13 种类型

| AppCompat文字样式                                            | MDC-Android文字样式                           | MDC-Android文字属性       |
| ------------------------------------------------------------ | --------------------------------------------- | ------------------------- |
| `TextAppearance.AppCompat.Display4`                          | `TextAppearance.MaterialComponents.Headline1` | `textAppearanceHeadline1` |
| `TextAppearance.AppCompat.Display3`                          | `TextAppearance.MaterialComponents.Headline2` | `textAppearanceHeadline2` |
| `TextAppearance.AppCompat.Display2`                          | `TextAppearance.MaterialComponents.Headline3` | `textAppearanceHeadline3` |
| `TextAppearance.AppCompat.Display1`                          | `TextAppearance.MaterialComponents.Headline4` | `textAppearanceHeadline4` |
| `TextAppearance.AppCompat.Headline`                          | `TextAppearance.MaterialComponents.Headline5` | `textAppearanceHeadline5` |
| `TextAppearance.AppCompat.Title` `TextAppearance.AppCompat.Large` | `TextAppearance.MaterialComponents.Headline6` | `textAppearanceHeadline6` |
| `TextAppearance.AppCompat.Subhead` `TextAppearance.AppCompat.Menu` | `TextAppearance.MaterialComponents.Subtitle1` | `textAppearanceSubtitle1` |
| `TextAppearance.AppCompat.Small`                             | `TextAppearance.MaterialComponents.Subtitle2` | `textAppearanceSubtitle2` |
| `TextAppearance.AppCompat.Body1`                             | `TextAppearance.MaterialComponents.Body1`     | `textAppearanceBody1`     |
| `TextAppearance.AppCompat.Body2`                             | `TextAppearance.MaterialComponents.Body2`     | `textAppearanceBody2`     |
| `TextAppearance.AppCompat.Button`                            | `TextAppearance.MaterialComponents.Button`    | `textAppearanceButton`    |
| `TextAppearance.AppCompat.Caption`                           | `TextAppearance.MaterialComponents.Caption`   | `textAppearanceCaption`   |
| 不适用                                                       | `TextAppearance.MaterialComponents.Overline`  | `textAppearanceOverline`  |

#### 例子

```xml
<com.google.android.material.card.MaterialCardView
    ...>
    ...
    <TextView
        android:id=”@+id/headerText”
-        android:textAppearance="@style/TextAppearance.AppCompat.Title"
+        android:textAppearance="?attr/textAppearanceHeadline6"
        ... />
    <TextView
        android:id=”@+id/subheadText”
        android:textColor="?android:attr/textColorSecondary"
-        android:textAppearance="@style/TextAppearance.AppCompat.Body2"
+        android:textAppearance="?attr/textAppearanceBody2"
        ... />
    <TextView
        android:id=”@+id/supportingText”
        android:textColor="?android:attr/textColorSecondary"
-        android:textAppearance="@style/TextAppearance.AppCompat.Body2"
+        android:textAppearance="?attr/textAppearanceBody2"
        ... />
</com.google.android.material.card.MaterialCardView>
```

#### 自定义

我们还可以选择在应用程序主题中覆盖类型比例，以使用自定义字体系列，[XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml)或通过Android Studio [下载](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts)字体：

```xml
<!-- Copyright 2020 Google LLC.
   SPDX-License-Identifier: Apache-2.0 -->
<style name="Theme.App" parent="Theme.MaterialComponents.*">
    ...
+    <item name="textAppearanceHeadline6">@style/TextAppearance.App.Headline6</item>
+    <item name="textAppearanceBody2">@style/TextAppearance.App.Body2</item>
</style>

+<style name="TextAppearance.App.Headline6"
+    parent="TextAppearance.MaterialComponents.Headline6">
+    <item name="fontFamily">@font/roboto_mono_medium</item>
+</style>
+<style name="TextAppearance.App.Body2"
+    parent="TextAppearance.MaterialComponents.Body2">
+    <item name="fontFamily">@font/roboto_mono_regular</item>
+</style>
```

上面我们只是重写了 13 种类型中的一种。如果你想要改变字体的话，建议也把剩余的 12 修改了，以保持APP中字体的一致性。

### Shape

`ShapeAppearance` styles/attributes 

Shape（ [Material Design shape system](https://material.io/design/shape/)） 是用来处理 MDC 控件的边角的一种方式，分成了小，中，大

这些合适的样式属性来自 `ShapeAppearance.*` styles。包括：cornerFamily  （两种值：rounded  cut） 。用 `cornerSize`  来表示尺寸

![](F:\MyFile\mybook\文章使用图片\MaterialDesign\shape.png)
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/98c24f07c05c4203ae9baca27927230f~tplv-k3u1fbpfcp-zoom-1.image)

MDC小部件使用这些属性来设置其背景样式。要了解哪些窗口小部件适用于哪些形状类别，需要检查[源代码中](https://github.com/material-components/material-components-android)的默认窗口小部件样式。

#### 控件背景

实现此功能的类为 `MaterialShapeDrawable`. 默认情况下，所有的 MDC 控件都将此可绘制对象当做背景，我们也可以考虑将它用作自定义 View 的背景。它可以处理形状主题、阴影、黑色主题等等。

因此。我们不建议使用 `android:background` 作为 MDC 控件的背景。因为它会覆盖 `MaterialShapeDrawable`。大多数的  MDC 控件的默认 style 都指定了  `<item name="android:background">@null</item>`

为了避免这种情况，应该使用 `shapeApperance/shapeAppearanceOverlay` 和 `backgroundTint` 属性来调整背景形状和颜色。

以下情况需要单独注意：

- `MaterialButton` 在 `1.2.0-alpha06` 版本前忽略了 `android:background` 如果你确实需要用这个属性，考虑使用  `AppCompatButton` 在你的布局中。
- `MaterialShapeDrawable` 是不支持 `gradients` 的。如果确实需要的话，最好用 `android:background`

#### 例子

在我们的示例中我们可以删除一些由 `shape theming` 来处理的属性。

```xml
<com.google.android.material.bottomnavigation.BottomNavigationView
-    android:background="@android:color/white"
    ... />

<com.google.android.material.card.MaterialCardView
-    app:cardCornerRadius="2dp"
    ...>
    ...
</com.google.android.material.card.MaterialCardView>
```

#### 使用 `corner family` 和 `size` 来自定义 shape

我们可以选择在应用主题中覆盖形状样式来表达我们自己的品牌。

```xml
<style name="Theme.App" parent="Theme.MaterialComponents.*">
    ...
+    <item name="shapeAppearanceSmallComponent">@style/ShapeAppearance.App.SmallComponent</item>
+    <item name="shapeAppearanceMediumComponent">@style/ShapeAppearance.App.MediumComponent</item>
+    <item name="shapeAppearanceLargeComponent">@style/ShapeAppearance.App.LargeComponent</item>
</style>

+<style name="ShapeAppearance.App.SmallComponent"
+   parent="ShapeAppearance.MaterialComponents.SmallComponent">
+    <item name="cornerFamily">rounded</item>
+    <item name="cornerSize">8dp</item>
+</style>
+<style name="ShapeAppearance.App.MediumComponent"
+    parent="ShapeAppearance.MaterialComponents.MediumComponent">
+    <item name="cornerFamily">rounded</item>
+    <item name="cornerSize">12dp</item>
+</style>
+<style name="ShapeAppearance.App.LargeComponent"
+    parent="ShapeAppearance.MaterialComponents.LargeComponent">
+    <item name="cornerFamily">rounded</item>
+    <item name="cornerSize">16dp</item>
+</style>
```

![](F:\MyFile\mybook\文章使用图片\MaterialDesign\shape.gif)
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c623add9f5584db09250a23b55406654~tplv-k3u1fbpfcp-zoom-1.image)

使用 shape theming 的例子

恢复 Button 的自定义渐变背景

```xml
-<Button
+<androidx.appcompat.widget.AppCompatButton
    android:background="@drawable/bg_button_gradient"
+    android:textAppearance="?attr/textAppearanceButton"
    ... />
```

如果使用的是 `MDC 1.2.0-alpha-06` 或者更新的版本，可以直接使用 `MaterialButton` 的 `android:background`。需要注意的是要清空 `backgroundTint`，因为在默认的 style 中，`backgroundTint` 为 `colorPrimary` 

```xml
<!-- Copyright 2020 Google LLC.
   SPDX-License-Identifier: Apache-2.0 -->
<Button
    android:background="@drawable/bg_button_gradient"
+    app:backgroundTint="@null"
    ... />
```