---
layout: post
title:  "Activity 从启动到布局绘制的简单分析"
date:   2018-12-10 15:14:54
categories: Android
tags: android Activity
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}    









这篇文章主要是配合源码简单的介绍一下，程序的加载过程，Activity 中布局的加载过程，能够大体的了解整个过程。不过过度的追究细节，因为里面任何一个细节可能都够你研究一段时间的！先了解掌握大体过程，再慢慢来！

>文章最早发布于我的微信公众号  **Android开发者家园**  中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！   


## 开始启动

我们都知道，Activity 是有生命周期的，`onCreate()`、`onStart()` 、`onResume` 等等那么这些方法是如何调用的呢？它不会平白无故的自己调用。其实当我们开启 APP 的时候会创建一个叫做 ActivityThread 的类，我们可以认为这个类是主类，就和 Java 程序中的启动类一样，ActivityThread 类有一个 `main` 函数，这个函数就是我们的程序入口！      

![main.jpg](https://upload-images.jianshu.io/upload_images/6737388-51e66f695c2ea472.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

相信看到这个，大家都会恍然大悟了，这就是我们学习 Java 的时候的 `main` 方法，认为是程序的入口。可以看到方法里面对一个 `Looper` 对象进行了初始化，`Looper.prepareMainLooper()` 通过这个方法就给当前的线程初始化了 Looper，这个 Looper 成了 Application 的 main looper，这也是为什么我们在主线程中不用自己再 Looper.prepare 的原因。可以认为任何在主线程的操作都会发送到这个 `Looper` 对应的 `Handler` 中去。很容易可以找到 `Looper` 对应的 `Handler`，其实就是 ActivityThread 的一个内部类，继承了 `Handler` 

![handler-1.jpg](https://upload-images.jianshu.io/upload_images/6737388-7b58e89d2208149d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![handler-2.jpg](https://upload-images.jianshu.io/upload_images/6737388-8fbcec230c0e541d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个就是 `ActivityThread` 中 `Handler` 的部分代码实现，然后看到 Handler 里面的这段代码

```java
public void handlerMessage(Message msg){
    .....;
    switch(msg.what){
        case LAUNCH_ACTIVITY:
            .....;
            // r 是在 msg 里面拿到的对象 msg.obj
            handlerLaunchActivity(r,null,"LAUNCH_ACTIVITY");
            .....;
    }
}
```

下面再来看 `handlerLaunchActivity()` 这个方法：

```java
private void handleLaunchActivity(ActivityClientRecord r,Intent customIntent,String reason){
    ·····;
    Activity a = performLaunchActivity(r,customIntent);
    ......;
   	handlerResumeActivity(...);
    
}
```

里面这两个很重要的方法我已经列出来了，下面我们来看看 `performLaunchActivity(r,customIntent)` 方法：

```java
// 这里面代码同样很多，我们只挑选重要的
private Activity performLaunchActivity(ActivityClentRecord r,Intent customIntent){
    .......;
    // 这里 new 出了 activity
    activity = mInstrumentation.newActivity(cl,component.getClassName(),r.intent);
    ......;
    // 调用了 Activity 的 attach 方法（很重要）
    activity.attach(appContext,this,getI......);
    ......;
    // 调用了 Activity 的 onCreate 方法
    mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
    // 调用了 onStart 方法
    ........;
}
```

这段代码中分别调用了 Activity 的 attach 方法和 onCreate 方法，同时下面也调用了 onStart 方法，这里省略了。

**结论：**在 performLaunchActivity 方法中，首先通过 mInstrumention 产生了 Activity 对象，然后调用了 Activity 的 attach 方法，然后通过 Instrumentation 对象调用了 activity 的生命周期中的方法。说了这么多，无非是大体了解了这个启动过程。知道 Activity 在执行生命周期前是先调用 attach 方法的。其中 attach 方法内的一些代码是很关键的，和整个 Activity 的启动有很重要的关系，下面来看一下 attach 方法的源码：

```java
// 这个方法存在于 Activity 类中
final void attach（Context context，ActivityThread aThread,Instrumentation instr,IBinder token,int ident,Application application,Intent intent,ActivityInf info,CharSequence title,Activity parent,String id,.........）{
    
    .........;
    // Window 对象赋值
    mWindow = new PhoneWindow(this,window,activityConfigCallback);
    mWindow.set 许多监听回调 (WindowContrallerCallBack、Callback)等等;
    将各种传递过来的各种参数赋值给 Activity 中的成员；
          				     mWindow.setWindowManager((WindowManager)context.getSystemService(Context.WINDOW_SERVICE),mToken,mComponent.flattenToString(),(info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED)!=0);
    mWindowManager = mWindow.getWindowManager();
    
}
```

**attach 这个方法的主要内容**：首先给 Activity 中的 mWindow 成员变量赋值，**具体的实现对象是 PhoneWindow**，然后给 Activity 中的其他许多关键的成员赋值，给 mWindow 变量设置 WindowManager，然后给 Activity.mWindowManager 赋值。**mWindow 是一个 Window 类型的变量，实际上一个 PhoneWindow 对象，和 Activity 的内容显示有关系**    

## onCreate

下面就开始执行 Activity 中的 **onCreate** 方法了。

我们都知道，每一个 Activity 对应一个布局文件，在 Activity 的 `onCreate()` 方法中都需要 `setContentView(int res)` 通过这个方法我们就可以将布局与 Activity 关联起来了。那么布局是怎么加载的呢。这个时候就要看 `setContentView()` 的源码了：

```java
public void setContentView(@LayoutRes int layoutResID){
    getWindow().setContentView(layoutResID)；
    initActionBar（）；    
}
```

getWindow() 返回的是 Window 对象，这个对象刚刚在 attach 方法中我们接触过，其实它的真正实现是 PhoneWindow，下面来看 PhoneWindow 中的 setContentView 方法：

```java
// 放置窗口内容的视图，它既可以是 mDecor 本身（没有 Title 的情况下），也可以是 mDecor 的子项， 这种情况就是 mDecor 中有 Title 的情况
ViewGroup mContentParent;
// 这是窗口的顶层视图，包含窗口装饰
private DecorView mDecor;

public void setContentView(int layoutResID){
    if(mContentParent == null){
        installDecor();
    }else if(!hasFeature(FEATURE_CONTENT_TRANSITIONS)){
        mContentParent.removeAllView();
    }
    // 将 布局资源 layoutResID 填充到 mContentParent 中
    mLayoutInflater.inflate(layoutResID,mContentParent);
    ........；
}
```

PhoneWindow 类中有两个和视图相关的成员变量，一个是 DecorView mDecor，另一个是 ViewGroup mConentParent    

下面再来看看 PhoneWindow 中 setContentView 中的 inStallDecor() 方法：

```java
private void installDecor(){
    ......;
    
    if(mDecor == null){
        // 生成 mDecor;
        mDecor = generateDecor(-1);
        ......;
    }else{
        mDecor.setWindow(this);
    }
    
    if(mContnetParent == null){
        mContentParent = generateLayout(mDecor);
        .......;
        ....很多代码;
        // 可以认为 DecorContentParent 是操作 Title 的
        final DecorContentParent decorContentParent = (DecorContentParent)mDecor.findViewById(R.id.decor_content_parent);
        if(decorContentParent != null){
            mDecorContentParent = decorContentParent;
            .......;
            对 Title 的一系列操作;
        }
        // 也有获取 TitleView 的代码
       
    }
        
}

```

mDecor 是通过 generateDecor() 方法来获取的。

```java
protected DecorView generateDecor(int featureId){
    ......;
    return new DecorView(context,featureId,this,getAttributes());
    
}
```

DecorView 继承了 FrameLayout 是整个 PhoneWindow 的根视图   

再来看看 generateLayout（DecorView decor）方法：

```java
protected ViewGroup generateLayout(DecorView decor){
	// 获取当前主题中的一些属性
    TypedArray a = getWindowStyle();
    // getWindowStyle() 的内容：mContext.obtainStyledAttributes(com.android.internal.R.styleable.Window);
    // 其实就是获得定义的属性组中的一些信息
    ......;
    // 省略的代码大概就是利用 a 来获取主题中一些默认的样式，把这些样式设置到 DecorView 中
    // 大概内容,类似于
    mIsFloating = a.getBoolean(R.styleable.Window_windowIsFloading,false);
    if(mIsFloating){
        setLayout(WRAP_CONTENT,WRAP_CONTENT);
    }
    // 有没有 windowNoTitle 等等这种属性
    // 这里的设置大小，可以理解为是设置 DecorView 大小，在这里已经确定了 DecorView 的大小了
    // 根据SDK 版本来设置视图样式代码
    
    // 填充窗口的资源
    int layoutResource;
    .....根据不同的条件，给 layoutResource 赋予不同的值;
    
    mDecor.startChanging();
    // 资源填充到 mDecor 中
    mDecor.onResourcesLoaded(mLayoutInflater, layoutResource); 
    // 这里的 findViewById 就是在 mDecor 中寻找
    ViewGroup contentParent = (ViewGroup)findViewById(ID_ANDROID_CONTENT);
    ....修改 mDecor 背景颜色，设置 Title 的一些属性;
    return contentParent;
    
}
```

可以看到 generateLayout() 方法里面还是处理了许多关键的事情： 

- 根据各种属性值设置了 LayoutParam 参数
- 根据 FEATURE 的值，选择了合适的布局资源 id 赋值给了 layoutResource。
- 根据 Theme 中设置的不同的风格，SDK 不同的风格来对 DecorView 进行设置，Decor 装饰的意思，所以这个名字也很符合。
- 把 layoutResource 填充到了 DecorView 中
- 在 layoutResource 中有一个 id 为 ID_ANDROID_CONTENT 的 ViewGroup 即我们的 contentParent
- DecorView 包含了 Title 和 contentParent          

## installDecor() 小结：

1. 一个 Activity 对应了一个 Window，这个 Window 的实现对象是 PhoneWindow，一对一的关系
2. PhoneWindow 管理了整个 Activity 页面内容，**不包括系统状态栏** ，PhoneWindow 是和应用的某个页面关联的。
3. PhoneWindow 包含 ActionBar 和 内容。setContentViwe 方法是用来设置内容的，setTitle 是用来操作 Title 的。Window 中定义的 requestFeature() 等方法，很多与 ActionBar 属性相关的设置。这些方法都是公共方法，是为了方便我们调用的。
4. PhoneWindow 本身不是一个视图，它的成员变量 mDecor 是整个页面的视图。mDecor 是在 generateLayout() 的时候填充的。Title 和 contentParent 都是通过 findViewById() 从 mDecor 中获取的。

上面介绍的这些，只是执行完了 `installDecor()` 方法，这个时候，PhoneWindow 有了 DecorView，DecorView 有了自己的样式，有了 Title 和 ContentParent。下一步就是向 ContentParent 中添加自己的布局了。

```java
public void setContentView(int layoutResID){
    .....;
    // 上面已经分析完了
    installDecor();
    // 向 ContentParent 中添加我们自己的布局资源
    mLayoutInflater.inflate(layoutResID,mContentParent);
    .......;
}
```

到此 setContentView 算是分析完了，onCreate 也就执行完了。那么我们可以认为上面的 ActivityThread 中的 performLaunchActivity() 执行完了，接下来就开始执行 handleResumeActivity() 方法了。

```java
final void handleResumeActivity(IBinder token,boolean clearHide,boolean isForward,boolean reallyResume,int seq,String reason){
    ......;
    // 可以把 ActivityClientRecord 类认为是记录 Activity 内部关键参数的类
    ActivityClientRecord r = performResumeActivity(token,clearHide);
    if(r!=null){
        final Activity a = r.activity;
        .....;
        if(r.window == null && !a.mFinished && willBeVisible){
            r.window = r.activity.getWindow();
            View decor = r.window.getDecorView();
            ....;
            ViewManager wm = a.getWindowManager();
            WindowManager.LayoutParams l = r.window.getAttributes();
            .....;
            if(a.mVisibleFromClient){
                a.mWindowAdded = true;
                wm.addView(decor,l)
            }
        }
    }
    。。。。。;
    
}
```

**performResumeActivity()**方法，这个方法内部调用了 Activity 的  performResume() 方法(这个方法在 API 中没有出现，需要在完整源码中查看) 

![performResumeActivity.jpg](https://upload-images.jianshu.io/upload_images/6737388-f3c4abca0a1c7906.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看到上面 `mInstrumentation.callActivityOnResume(this)` 就是调用了 Activity 的 onResume 方法。

然后再来看看 `hanleResumeActivity` 方法里面的 `addView` 方法，wm 是上面 a.getWindowManger() 获取到的，a 是 Activity，getWindowManager() 返回了 mWindowManager 对象，而这个对象是 WindowMangerImpl，它内部方法大部分是在 WindowManagerGlobal 内部实现

```java
private final ArrayList<ViewRootImpl> mRoots = new ArrayList<ViewRootImpl>();
private final ArrayList<WindowManager.LayoutParams> mParams = new ArrayList<>();

public void addView(View view,ViewGroup.LayoutParams params,Display display,Window parentWindow){
    .......;
    ViewRootImpl root;
    View panelParentView = null;
    ......;
    final WindowManager.LayoutParams wparams = (WindowManager.LayoutParams) params;
    
    root = new ViewRootImpl(view.getContext(),display);
    view.setLayoutParams(wparams);
    
    mViews.add(view);
    mRoots.add(root);
    mParams.add(wparams);
    try{
        root.setView(view,wparams,panelParentView);
    }catch (RuntimeException e){
        .....;
    }
}
```

上面代码中，在 addView 方法中，new 了一个 ViewRootImpl 对象，然后调用 ViewRootImpl.setView() 方法。

```java
/**
 * We have one child
 */
public void setView(View view,WindowManager.LayoutParams attrs,View panelParentView){
    synchronized(this){
        if(mView == null){
            mView = view;
            
            mAttachInfo.mDisplayState = mDisplay.getState();
            mDisplayManager.registerDisplayListener(mDisplayListener,mHandler);
            ....;
            requestLayout();
            .....;
            view.assignParent(this);
            .....;
        }
    }
    
}

```

首先将传进来的 view 赋值给 mView，然后调用了 requestLayout() 方法，ViewRootImpl 不是 View 的子类，可以认为 ViewRootImpl 是这个 Activity 所对应的整个布局的根，它来执行具体的绘制开始，view.assignParent(this)，就是给自己分配了 Parent，Parent 就是 ViewRootImpl 对象。

```java
public void requestLayout(){
    .....;
    
    scheduleTraversals();
}
```

最终调用了 `performTraversals()` 方法，performTraversals 方法内部代码很多

这里只写一下重要的部分

```java
// 不传递参数，默认是 MATCH_PARENT
final WindowManager.LayoutParams mWindowAttributes = new WindowManager.LayoutParams();
// window 可以使用的宽度
int mWidth;
// window 可以使用的高度
int mHeight;

private void performTraversals(){
    int desiredWindowWidth;
    int desireWindowHeight;
    WindowManager.LayoutParams lp = mWindowAttributes;
    
    // 获取应该给根 View 多大
    // mWidth 就是我们窗口的大小，lp.width 始终是 MATCH_PARENT
    int childWidthMeasureSpec = getRootMeasureSpec(mWidth,lp.width);
    int childHeightMeasureSpec = getRootMeasureSpec(mHeight,lp.height);
    
    // 告诉 根 View 多大
    performMeasure(childWidthMeasureSpec,childHeightMeasureSpec);
    // perforMeasure 内部实现很简单
    
    performLayout(lp,mWidth,mHeight);
    
    performDraw();
    
    
}
```

调用 `getRootMeasureSpec(mWidth,lp.width)` 得到 childWidthMeasureSpec ：

![getRootMeasureSpec()-1.jpg](https://upload-images.jianshu.io/upload_images/6737388-2044d14935ac72c6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![getRootMeasureSpec()-2.jpg](https://upload-images.jianshu.io/upload_images/6737388-911d480b591da0ed.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过这个方法就获取了子 View 应该是多大，还有呈现的模式。然后把得到的数值，通过 performMeasure() 方法设置 view 大小。performMeasure 方法：

```java
private void performMeasure(int childWidthMeasureSpec,int childHeightMeasureSpec){
   .....;
    // 这里的 mView 就是 PhoneWindow 中的 DecorView
   mView.measure(childWidthMeasureSpec,childHeightMeasureSpec);
}
```

通过这一步：mView.measure(childWidthMeasureSpec,childHeightMeasureSpec) 就是给 mView 确定大小值了，也就是根 View。

这样整个 Activity 的布局对应的根 View---DecorView 的大小就确定了。具体来看看 mView.measure()：

```java
// 测量一个 View 应该是多大的，参数是由它的父 View 提供的，widthMeasureSpec
// 包含了大小和约束条件
public final void measure(int widthMeasureSpec,int heightMeasureSpec){
    .......;
    onMeasure(widthMeasureSpec,heightMeasureSpec);
    .......;
}
```

该方法又调用了`onMeasure` 方法：

```java
// 仅仅是 View 里面的 onMeasure 方法，不同的 View 子类有不同的实现内容
protected void onMeasure(int widthMeasureSpec,int heightMeasureSpec){
    setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(),widthMeasureSpec),getDefaultSize(getSuggestedMinimumHeight(),heightMeasureSpec));
}

// 官网文档：测量 View 的大小，这个方法通过 measure 方法来调用，View 的子类应该重写此方法根据子 View 的特点。在重写这个方法的时候，你必须调用 setMeasuredDimension(int,int) 来储存测量出来的宽度和高度。
```

下面就是调用 `setMeasuredDimension()` 了。其中最关键的一步就是对 View 的两个成员变量进行了赋值（在 `setMeasuredDimensionRaw()`） 方法中实现的   

```java
// 这个方法必须在 onMeasure() 方法中被调用用来储存测量出的宽度和高度。
protected final void setMeasuredDimension(int measuredWidth,int measuredHeight){
    ......;
    // 为了方便，这里直接把 setMeasuredDimensionRaw() 方法内容写到下面了
    //给 View 的成员变量赋值
    mMeasuredWidth = measureWidth;
    mMeasuredHeight = measuredHeight;
    
    mPrivateFlags |= PFLAG_MEASURED_DIMENSION_SET;
    
}

```

好了到此 View 中的 measure 方法也介绍完毕了。是不是觉得 onMeasure 方法里面很简单啊！这你就错了，不要忘了我们分析的 onMeasure 方法是分析的类 View 中的。里面注释明确写明了 **View 的子类必须根据自身需求来重写 onMeasure 方法**  。我们上面分析的只是一个过程。不要忘了我们的根 View 是 DecorView，而 DecorView 的父类又是 FrameLayout。可以到这两个具体的对象中看看他们的 onMeasure。感兴趣的可以看一下源代码，其实分析到这里就可以得出结论了(这个结论仅仅是 measure 这一部分的结论) 后面再从 Activity 的启动一块串联起来！ 

## Measure 结论

Activity 有一个祖先 View 即 DecorView，通过前面的分析 DecorView 的大小是由 ViewRootImpl 中给赋值的，我们可以认为 ViewRootImpl 是所有 View 的根，我们知道我们布局是呈现树的结构。我这里有一个比喻：ViewRootImpl 是这颗树的根，是埋在地下面的，DecorView 是树的主干是在地上面的，ViewGroup 是枝干，单个的 View （类似于 TextView、Button 这种）是树叶。DecorView 这个主干上长出许多枝干，这里我们的这棵树有两个重要的枝干（或者一个），这个需要根据树的种类样式来决定。这两个重要的枝干就是：Title 和 mContentParent，或者只有 mContentParent。然后这个两个枝干又会生长出许多的枝干，枝干上面生长树叶。 

measure() 方法是由其父 View 来调用执行。从根上追溯，DecorView 的大小是由树根（ViewRootImpl）来赋值的，然后枝干又是由 DecorView 来调用的 measure 的。一层层的调用。

举个简单栗子：

```xm
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:id="@+id/ll_parent"
              android:layout_width="match_parent"
              android:layout_height="match_parent">
    <RelativeLayout
    	android:id="@+id/rl_parent_1"
        android:layout_width="match_parent"
        android:layout_height="200dp">
        <TextView
            android:id="@+id/tv_child_1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/view"/>
        <TextView
            android:layout_below="@id/tv_child_2"
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:text="@string/activity_cycle"
            />
    </RelativeLayout>
    <LinearLayout
    	android:id="@+id/ll_parent_1"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <EditText
        	android:id="@+id/et_child_1"
            android:inputType="text"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:hint="@string/progress_bar"/>
    </LinearLayout>
</LinearLayout>
```

我们这里有这样一棵树，上面的布局是我们这颗树的 mContentParent 。首先 ViewRootImpl 会给 DecorView 大小赋值（具体大小是由 generateLayout 的时候确定的）。然后 DecorView 会调用 mContentParent 的 measure ，传入 DecorView 允许它的大小。当然具体的大小是由 measure 传入的参数和 mContentParent 共同决定的（具体细节下面介绍）然后 mContentParent 再调用 ll_parent.measure() 给它传入 mContentParent 所允许它的大小。这个时候就会激活 ll_parent 的 onMeasure 方法，在 ll_parent 的 onMeasure 方法里面肯定会调用 rl_parent_1.measure 方法，然后激活 rl_parent_1 的 onMeasure 方法，在 onMeasure 方法里面调用 tv_child_1.measure ，tv_child_1 没有孩子了，直接设置自己的大小。然后再一层层的向父布局返回去。大体就是这样的一个过程。  

当然 measure(int widthMeasureSpec,int heightMeasureSpec) 这里的参数包含了，子元素的大小的属性和允许子元素的大小。具体可以看 MeasureSpec 类，很简单。

到此每个布局就知道自己的大小了。然后开始执行 ViewRootImpl.performLayout() 方法了   

## Layout()

其实这个和 measure() 方法相似。都是在父控件中调用 layout() 方法，然后在 layout 方法中会调用 onLayout 方法。和 measure 不同的是 onLayout 方法是给自己的子 View 来布局的，如果不是 ViewGroup 就不需要重写 onLayout 方法了。measure 的宽度和高度是该控件期望的尺寸，真正在屏幕上的位置和大小是由 layout 来决定的。接下来就是递归调用他们的 onLayout 方法。

接下来就是开始 Drawa 了，介绍到这里整个过程算是完成了。

参考：https://www.cnblogs.com/kross/p/4029570.html

> 个人水平有限，如有纰漏欢迎指正！


![欢迎大家关注我的微信公众号，和我交流分享](http://upload-images.jianshu.io/upload_images/6737388-1eca35c3d7e04a1e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 






















