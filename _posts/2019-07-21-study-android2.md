---
layout: post
title:  "从0系统学Android-2.1Activity的使用"
date:   2019-07-22 15:14:54
categories: 从0系统学Android
tags: Android
excerpt:  
mathjax: true
author: sydMobile
---
* content
{:toc}














![f](https://github.com/sydmobile/sydmobile.github.io/blob/master/pic/%E5%A4%B4%E5%9B%BE%E7%89%87%E4%B8%A2%E5%A4%B1.png?raw=true)

[更多精品文章分类](https://mp.weixin.qq.com/s/B8DP0UMg1fup2_sJVtgjMw)    

## 第二章：先从看的到的入手—Activity

上一章成功创建了自己的第一个项目。这一章从页面入手，来进行学习。

### 2.1 Activity 是什么

Activity 是一种可以包含用户界面的组件，主要用于和用户进行交互。一个应用可以有零个或者多个 Activity。

### 2.2 Activity 的基本用法

自己手动创建一个没有 Activity 的新项目

#### 2.2.1 手动创建 Activity

项目创建成功后，看到如下目录


![初始化项目.png](https://upload-images.jianshu.io/upload_images/6737388-2297c19414412bb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


自己手动创建一个 Activity

右击 包名`com.example.firstcode` —>`New`—>`Activity`—>`Empty Activity` 

这个时候会弹出一个对话框，然后不要选择 `Generate LayoutFile` 和`Launcher Activity`，然后点击 `Finish`


![创建Activity.png](https://upload-images.jianshu.io/upload_images/6737388-dcfdbcf721143b06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果勾选了 `Generate Layout File` 后会自动给我们的 Activity 创建一个 Layout 布局，勾选了 `Launcher Activity` 后表示自动将当前 Activity 设置为主 Activity。这些内容留给我们后面自己实现。

好了现在我们自己创建了一个 Activity 了，任何 Activity 都要重写 onCreate 方法。

```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle saveInstanceState){
        super.onCreate(savedInstanceState);
    }
}
```

可以看到刚刚生成的 Activity 的代码内容很简单，在 `onCreate`方法中就是调用了父类的 `onCreate` 方法，这是默认的实现方式，后面很多的代码还需要我们自己来慢慢填满！

#### 2.2.2 创建和加载布局

Android 的程序设计是讲究逻辑和视图分离的，最好要做到每个 Activity 都对应一个布局，布局是专门用来显示界面内容的。下面我们就来创建一个布局。

右击 `app/src/main/res` 目录——>New----->Directory，会弹出一个
新建目录的窗口，先创建一个 `layout` 的目录。然后对 `layout` 目录右键—>New-->Layout resource fie，这个时候就会弹出新建布局资源文件的窗口。

![first_layout.png](https://upload-images.jianshu.io/upload_images/6737388-883b0d77d060e166.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里我们这样取名`fist_layout` Root element 选择 `LinearLayout` 然后点击 OK

![布局编辑器.png](https://upload-images.jianshu.io/upload_images/6737388-3e75f655e7698062.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




看到图中的布局编辑器，这是 Android  Studio 为我们提供的可视化布局编辑器，在这里我们可以浏览我们布局的样子。在窗口下方有两个切换卡，左边是 `Design` 右边是 `Text` 。`Design` 是可视化布局编辑器，这这里可以预览布局，也可以通过拖拽编辑布局。`Text` 是通过 xml 来编辑布局。下面切换到 Text 模式。

![布局编辑器_text.png](https://upload-images.jianshu.io/upload_images/6737388-531c8736432f36f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


左边就是我们的布局代码部分，我们刚刚创建的时候选择了 `LinearLayout` 布局作为根布局，所以这里就是`LinearLayout` 。 下面对布局添加一个按钮。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:text="Button"
        android:id="@+id/bt"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```

简单介绍一下 Button 里面的几个属性：

- android:id 是给当前元素定义个唯一标识符，以便之后可以对这个元素在代码中进行操作。在 xml 中定义一个 id 的格式是：`android:id=@+id/id_name` 引用的格式`@id/id_name`
- Android:layout_width 指定当前元素的宽度。`match_parent`表示让当前元素和父元素一样宽。
- android:layout_height 指定当前元素高度。`wrap_content` 表示高度刚好可以包裹元素内容。
- android:text 指定了元素的显示内容。

关于布局的编写详细内容，会在下一章内容重点讲解。


![预览当前布局.png](https://upload-images.jianshu.io/upload_images/6737388-9823c2732651c2bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


预览一下这个布局，有可能你的和我的不太一样，没关系只要你的按钮显示出来就可以了。不一样的原因是我们的样式选择不一样。你可以看到上面有个 `Light` 这个是用来选择样式的，还有 `小眼睛的标志` 也有可能导致我们的预览效果看上去不一样。（这些东西在初学阶段不用去纠结）

布局已经加载出来了，下面我们要做的就是让 Activity 去加载这个布局。

回到 `MainActivity` 在 `onCreate` 方法中添加: 

```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 用于给当前 Activity 加载一个布局
        setContentView(R.layout.first_layout);
    }
```

我们在 `onCreate` 方法中添加了 `setContentView()` 方法来加载一个布局，需要传入一个布局的 id。**Android 项目中任何的资源（res）包中的东西都会在 R 文件中生成一个对应资源的 id** 因此我们可以通过 id 就可以将我们刚刚创建的布局加载到 Activity  中 了。

#### 2.2.3 在 AndroidManifest 文件注册

任何 Activity 都需要在 `AndroidManifest.xml` 中注册才可以使用。实际上我们的 `MainActivity` 已经在 `AndroidManifest` 中注册了。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.firstCode">

    <application
        android:allowBackup="false"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        tools:ignore="GoogleAppIndexingWarning">
        <activity android:name="com.example.firstcode.MainActivity"></activity>
    </application>

</manifest>
```

这是我们在创建 Activity 的时候，开发工具 Android Studio 自动给我们注册的。

注册方式：在 `<application>` 标签内容，通过 `<activiy>` 标签来注册。

介绍一下 `<activity>` 中的属性，通过 `android：name ` 来指明是哪一个 Activity。仅仅这样注册是不行的，因为没有给程序配置一个主 Activity，这样程序在运行的时候就不知道首先启动哪一个 Activity。

配置主 Activity 的方式：

在 `activity` 标签内添加`<intent-filter>` 标签。如

```xml
<intent-filter>
	<action android:name="android.intent.action.MAIN"/>
  <category android:name = "android.intent.category.LAUNCHER"/>
</intent-filter>
```

这样就会指定当前 Activity 为主 Activity。

除此之外，使用 `android:label` 属性来指定 Activity 的标题栏中的内容，标题栏是显示在 Activity 最顶部的（当然可以去掉）。**注意：**给主 Activity 设置 label 后，启动器中应用程序显示的名称也会是这个。

如果没有指定主 Activity 的话，程序是无法运行的。

![没有主Activity.png](https://upload-images.jianshu.io/upload_images/6737388-97357c321f769af8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


好了，经过上面的步骤后，我们就可以运行程序了。

![首次运行.png](https://upload-images.jianshu.io/upload_images/6737388-5840bdfc69db4896.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在页面最上面就是一个标题栏（如果你没有那是样式不一样，暂时忽略）。标题栏下面就是布局文件`first_layout` 编写的界面。

好了，现在我们已经掌握了如何创建一个 Activity 了下面继续学习我们在 Activity 中还可以做些什么！

#### 2.2.4 在 Activity 中使用 Toast

Toast 是 Android 系统中一种非常好的提醒方式，可以将一些短小的信息通知给用户，这些信息一段时间后会自动消失，并且不会占用任何屏幕空间。

要想使用 Toast ，首先我们需要有一个触发事件。我们就设置点击按钮的时候弹出 Toast 吧。在 `onCreate` 中添加代码：

```java
Button bt = findViewById(R.id.bt);
bt.setOnClickListener(new View.OnClickListener(){
  @Override
  public void onClick(View view){
    Toast.makeText(MainActivity.this,"显示内容",Toast.LENGTH_SHORT).show();
  }
});
```

在 Activity 中可以通过 `findViewById()` 方法来获取当前布局中的定义元素。传出的就是你想要获取的那个元素的 id。调用 `setOnClickListener`是给这个元素注册一个监听器，用于监听是否有点击。点击触发时就会调用 `onClick` 方法。

Toast 的使用方法很简单，通过调**用它的静态方法`makeText` 会创建一个 Toast 对象**，然后调用 `show` 方法，展示 Toast。makeText 方法需要传入三个参数，Context参数：上下文，Activity本身是Context 的子类。第二个参数就是要显示的内容，第三个参数是显示的时长，只能选择`Toast.LENGTH_SHORT` 或者 `Toast.LENGTH_LONG`。

运行结果：

![Toast运行结果.png](https://upload-images.jianshu.io/upload_images/6737388-33805e2a7d9e2251.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 2.2.5 在 Activity 中创建 Menu

手机不通与电脑，屏幕空间有限，需要充分的利用好手机空间，如果 Activity 中有大量的菜单，那么只是菜单就占据了大部分屏幕，是非常不合理的设计。Android 为了解决这个问题给我们提供了`菜单`

首先在 `res` 目录，新建一个 `menu` 目录，然后在 `menu` 目录内容新建一个 `menu resource file` ，取名 main。

然后在 `main.xml` 中添加代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/add_item" android:title="Add"/>
    <item android:id="@+id/remove_item" android:title="Remove"/>
</menu>
```

这里创建了两个菜单项，其中 `<item>` 标签就是来创建某一个菜单项的，然后通过属性 `android:id` 来给这个菜单项设置唯一标识，通过 `title`这个属性给菜单项指定名称。

然后到 Activity 中重写 `onCreateOptionsMenu()` 方法

```java
  @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main,menu);
        return true;
    }
```

通过 `getMenuInflater()` 获取 `MenuInflater` 对象，然后调用他的 `inflate` 方法。第一个参数就是指定我们创建的 Menu 的资源，第二个参数，用与指定我们的菜单将会添加到那个 Menu 对象中，这里直接使用方法中的 menu 然后 return true，表示运行菜单显示出来，如果是 false 则创建的菜单无法显示（三个点也没有）。

仅仅让菜单显示出来是不够的，我要对菜单进行操作，就用到方法 `onOptionsItemSelected()` 了。

```java
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()){
            case R.id.add_item:
                Toast.makeText(this,"add",Toast.LENGTH_SHORT).show();
                break;
            case R.id.remove_item:
                Toast.makeText(this,"remove",Toast.LENGTH_SHORT).show();
                break;
        }
        return true;
    }
```

在 `onOptionsItemSelected()`方法中通过 item.getItemId() 来判断我们点击了那个菜单项。

运行程序：

![menu.png](https://upload-images.jianshu.io/upload_images/6737388-432d92d885ccea84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


会发现在标题栏多了`三个点`，点击这三个点就会弹出菜单了。

![弹出菜单.png](https://upload-images.jianshu.io/upload_images/6737388-c65d680edc31fead.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




#### 2.2.6 销毁一个 Activity

想要销毁 Activity 很简单，Activity 给我们提供了一个方法`finish()` 我们只要调用一下这个方法，当前的 Activity 就会销毁。

![更多资料](https://upload-images.jianshu.io/upload_images/6737388-494987d373b04b64.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 