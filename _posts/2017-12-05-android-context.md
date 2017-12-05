---
layout: post
title:  "Android中Context的详细介绍"
date:   2017-12-05 15:14:54
categories: Android
tags: Context Android
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}







>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！
       
写过Java程序的都知道，我们在开发Java程序的时候，程序的入口在main()方法里面，在main()方法里面开始写我们程序就可以了，随便创建类就可以，但是在Android开发中，你见过 new Activity()、new Service()吗？没有把，这些Android中的组件不会像普通的Java对象一样new一下就可以创建实例然后自己调用相对应的方法执行了，这就是Android不同于普通的Java开发之处，因为Android系统给你定义了一个环境，这些组件都是在这个环境下运行的，有属于他们的生命周期，这些都是在Android系统中定义好的。这就是Android系统的环境，而这个环境中最重要的就是Context。
### Context的官方介绍
![Context介绍](http://img.blog.csdn.net/20171205093441808?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这是在Android开发者官网中关于Context的介绍，很简单的几句介绍。
连接着有关应用程序的全局信息，这是一个通过Android系统实现的抽象类，它允许访问特定应用程序的资源和类，以及对应用程序级的操作（比如启动活动，广播和接受意图等等）。它来允许获取以当前应用为特征的资源类型，是一个掌握着一些
资源（系统级别的,比如获取资源包里面的内容，图片，getSystemService等等一类）;是一个抽象类，Android系统提供了该抽象类的具体实现类（ContextImpl）；通过它我们就可以获取应用程序的资源和类（包括我们最常用的比如启动Activity，发广播等等）。
这样的描述是不是还是很抽象啊，慢慢往下来看。

先看一下Context类的继承关系
![context继承关系](http://img.blog.csdn.net/20171205094113142?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![context继承关系](http://img.blog.csdn.net/20171205094147274?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这是在Android Studio中查看的继承关系，在这个继承关系中，我把几个我们常用的圈了出来，Application、Activity、Service
这几个都是属于Context的子类。
Context的中文翻译为：上下文；语境；环境；背景，那么在我们开发中我们常常把他称为“上下文”，那么到底什么意思呢？我们在程序中可以这么理解，就是我们的当前对象（Activity、Server、Application等等这些都是对象）在这个程序中所运行的环境，也就是在整个程序中的环境，这些类都是在这个环境中进行运行，完成自己的生命周期的同样他们也都是这个环境中的一员。

这样抽象的介绍可能不利于理解，我来举一个栗子（可能会有点不恰当，但是会便于理解）：

通过API对Context的介绍，我们可以知道他掌握着应用程序的全局信息，通过他我们才可以访问使用应用程序的资源和类。把Android系统比如成一个公司，那么Context可以认为是这个公司的老板直接掌握着公司的资源，Application，Activity、Service等等其他的一些Context的子类，这些都是老板找的员工，有老板会给他们制定特定的工作和作息时间（对应它们的生命周期），这些员工可不是随随便便就找的都是需要系统（也就是公司）指定的负责特定的工作任务，所以不能像别的对象一样随便new一个，而TextView、View等等可以看做是这些主要员工工作的时候所需要的用品（比如：书本、桌子、电脑、鼠标等等一些工作辅助用品），相对来说可以根据所需的功能随便制定，需要了就可以new一个（相当于随便在市场上买一个）。这些所有的员工只有在老板的带领下，利用公司的资源才可以完成工作（开发一个APP）。这样的栗子应该会更加形象一些吧。   

为了便于理解我简单画了一下Context的继承关系图   

![Context继承关系图](http://img.blog.csdn.net/20171205095047826?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)       

当然这里画的只是重点把我们常用的类画了出来，Context的直接和间接的子类很多的。可能有人会说了为什么我在Android Studio中查看的时候没有看到ContextImpl呢，是这样的：  

![ContextImpl关系](http://img.blog.csdn.net/20171205095238039?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)         


![图片（ContextImpl）](http://img.blog.csdn.net/20171205095341567?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

上面是我在Android源码中的截图ContextImpl其实并不是以public class的形式存在，而是class继承了Context，这个文件是保护文件，就是注解了内部保护文件，所以我
们在IDE中是没有显示的，可以去源码中查看。

#### ContextImpl类介绍：

![图片（ContextImpl源码介绍） ](http://img.blog.csdn.net/20171205095535593?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

ContextImpl是Context API的常见实现，它为Activity和其他应用程序组件提供基本上下文对象，说的通俗一点就是ContextImpl实现了抽象类的方法，我们在使用Context的时候的方法就是它实现的。

#### ContextWrapper类介绍：

![图片（ContextWrapper源码介绍）](http://img.blog.csdn.net/20171205100355418?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

ContextWrapper类代理Context的实现，将其所有调用简单地委托给另一个Context对象（ContextImpl）,可以被分类为修饰行为而不更改原始Context的类，其实就Context类的修饰类。真正的实现类是ContextImpl,ContextWrapper里面的方法调用也是调用
ContextImpl里面的方法。

#### ContextThemeWrapper
就是一个带有主题的封装类，比ContextWrapper多了主题，它的一个直接子类就是Activity。

通过Context的继承关系图结合我们几个开发中比较熟悉的类，Activity、Service、Application,所以我们可以认为Context一共有三种类型，分别是Application、Activity和Service,他们分别承担不同的作用，但是都属于Context，而他们具有Context的功能则是由ContextImpl类实现的。

#### Context的功能
Context作为应用程序的运行环境，拥有的功能非常多，弹出Toast、启动Activity、启动Service、发送广播、操作数据库、获取资源文件等等还有许多都需要用到Context，由于Context的具体能力是由ContextImpl类去实现的，因此在绝大多数情况下，Activity、Service和Application这三种类型的Context是可以通用的，不过有几种场景比较特殊，比如启动Activity，还有弹出Dialog，这个时候由于安全等原因的考虑，Android不允许Activity或者Dialog凭空出现，一个Activity的启动必须要建立在另一个Activity的基础上，也就是以此形成的返回栈。而Dialog则必须在一个Activity上面弹出（除非是System Alert类型的Dialog），因此在这种场景下，我们只能使用Activity类型的Context，否则是会出错的。（其实使用Application类型的Context也是可以启动Activity的，但是不建议这么使用，会存在关于栈的问题）。
#### Context数量

上面已经说过了,Context在我们常用的类型中有Application、Activity、Service三种类型因此一个应用程序中Context的数量可以这么计算：  
Context数量 = Activity数量 +Service数量 + 1（Application数量）

（注意在多进程状态下Application的数量，网上有人说，有几个进程就会产生几Application实例，但是通过输入getApplicationContext的地址，我发现在不同进程中Application的地址是相同的，于是我认为多进程中Application其实还是一个，只是在新的进程中Application会重新调用onCreate（）进行实例化）  
### 分别分析一下Context的这几种类型
#### Application Context的设计
Application这个类也是很常见的，翻译过来就是应用程序，首先我们来看关于Application Android源码是怎么介绍的。

![（图片 Application源码）](http://img.blog.csdn.net/20171205101810183?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

维持着全局应用程序状态的基础类，你可以通过继承Application创建一个你自己的Application类。这个类要在AndroidMainfest.xml中的android：name属性中声明，并且这个类在任何其他类被实例化之前被实例化，换句话说Application这个类在我们打开应用程序的时候会被第一个实例化，Application是单例模式的，通过一些模块化的方式可以给你提供许多方法，如果你的单例模式需要一个全局的Context（比如注册broadcast receive的时候）
包括getApplicationContext（）作为一个参数可以调用你的单例的getInstance方法。   
下面进行代码测试：
我们新建一个自己的Application继承Application，并且在AndroidMainfest.xml文件中声明一下。

![（图片 Application声明）](http://img.blog.csdn.net/20171205102038383?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这样在我们启动我们的APP的时候Android系统就会首先为我们创建一个MyApplication我们在代码中是如何获取我们的Application实例的呢？其实很简单，Android API给我提供了getApplication方法来获取

![（图片==MainActivity_getApplication）](http://img.blog.csdn.net/20171205102148941?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这样我们就会获取我们的Application对象实例了，我们看一下打印结果：

![（图片==MainActivity_log）](http://img.blog.csdn.net/20171205102246514?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这是在MainActivity中的打印结果。这样看不刺激，我把与Context有关的几个方法都写上，我们来看一看结果（下面有具体的解释）

![（图片==Application相关方法）](http://img.blog.csdn.net/20171205102354311?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![（图片==MainActivity_log_内容）](http://img.blog.csdn.net/20171205102654324?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![（图片==ActivitySecond_log_全部内容）](http://img.blog.csdn.net/20171205102849989?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![（图片==ActivityThird_log_全部）](http://img.blog.csdn.net/20171205102927407?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

首先说明在这个程序是多进程，在启动ActivitySecond的时候启动了一个新的进程，也就是说ActivitySecond是属于com.syd.mystudydemo:second这个进程。     
从结果来看：
getApplication、getApplicationContext无论在哪个Activity中或者在不同的进程中得到的结果都是一样的。对应的都是MyApplicaton这个对象，不过需要注意的一点是在不同的进程中，开启新的进程的时候MyApplication是会重新调用onCreate方法的。这是需要注意的（有可能在不同进程中Application中的变量会赋予不同的值）。

那么getApplication和getApplicationContext有什么不同呢？
这两个方法所处的类不同，getApplicationContext的范围更大一点，它是在Context里面的方法，而getApplication这个方法，在Activity中。也就是所任何一个Context对象通过getApplicationContext都可以获取Context对象，而getApplication只是在Activity对象中可以获得。

![（图片==getApplicationContext—API说明）](http://img.blog.csdn.net/20171205103205403?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

getApplicationContext的API就说的很明白了，获取一个当前进程的Application对象，它应该被用在这种情况下，如果你需要这个Context对象和当前所在的context生命周期分离的话（也就是用getApplicationContext获取的Application生命周期和当前组件没有关系是和当前进程有关系的）用registerReceive（BroadcastReceiver,IntentFilter）来举个例子。如果你用一个来自Activity的Context（就是当前Activity，前面说了Activity是Context的子类）去注册一个receive的话，这个receiver是在这个Activity这个范围内的，这样的话就是意味着你在此Activity被销毁前就要unregister。事实上如果你不这样做的话，Android系统将会清除你泄露的注册，移除这个Activity然后log一个错误信息。因此，如果你使用Activity上下文来注册一个静态的接收者（对进程是全局的，而不是与一个Activity实例关联的），那么在你使用的活动被销毁的任何点上，你的注册将被删除。如果你使用getApplicationContext方法返回的Context，这个receiver被注册在全局状态在全局状态下和你的Application有有关联。因此它将永远不会被系统unregistered。在receiver与静态数据有关，而不是某一个特殊的组件的时候这样做是有必要的。然而使用getApplicationContext在别的地方是很容易引起内存泄露的如果你忘记了unregister，unbind，等等。  

所以我们可以知道使用getApplicationContext所获取的Context是和具体某一个组件是没有关系的，而他和你的整个进程有关系，作用的范围很大，但是如果你不计条件场景乱用的话，很容易造成内存的泄露。

![（图片==getApplication-API说明）](http://img.blog.csdn.net/20171205103818720?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
API说的很明确也很简单就是返回属于当前Activity的Application对象，这样也说明了getApplication是Activity类的方法。

![（图片==getContext--API说明）](http://img.blog.csdn.net/20171205105347407?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

返回基础context对象，通过构造方法或者setBaseContext设置值的context。   
其实获取的这个context对象就是ContextImpl对象，没错就是抽象类Context的实现类，其实Context里面的方法都是在ContextImpl里面完成的，而Application、Activity、Service这些Context的类型并没有实现Context里面的方法，而是调用的ContextImpl里面实现的方法。我们来看看代码是怎么实现的吧，Application、Service的父类就是ContextWrapper，而ContextWrapper就是Context的装饰类，它并没有具体的实现Context里面的抽象方法，而是这样调用的，上源码。

![（图片==ContextWrapper源码）](http://img.blog.csdn.net/20171205105516151?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![（图片==ContextWrapper源码）](http://img.blog.csdn.net/20171205105556999?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到ContextWrapper里面的方法的实现都是mBase.method()，其实调用的都是mBase里面的方法，而这个地方的mBase就是ContextImpl对象。

我们可以看一下这个方法

![（图片 ==ContextWrapper-att方法）](http://img.blog.csdn.net/20171205105644271?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

其实mBase都是通过这个方法来赋值的，而这个方法系统会自动调用，在onCreate之前就会调用。我们需要知道的是，类似于Application、Activity、Service这几种系统组件，不是通过构造方法new出来的，而是系统调用的，我们可以认为Application是在onCreate后便由Android系统生成了一个Application对象（单例，onCreate不会重复调用，除非多进程中）。

### 总结：
Context作为应用程序的运行环境，拥有的功能非常多，弹出Toast、启动Activity、启动Service、发送广播、操作数据库、获取资源文件等等这些方法都是在Context中的。

比如我们常见的方法： 
getResources(),getPackageManager(),getContentResolver(),getApplicationContext(),getColor(),getDrawable()，obtainStyledAttributes(),getPackageName(),getPackageResourcePath(),getSharedPreferences(),deleteFile(),getExternalCacheDir()       
等等方法，你可以这么想，Context表示Android系统中我们所编写的APP程序的运行环境，它掌握着App的运行资源，所有这些与我们APP程序有关的资源方法都在Context里面（这样我们在Context的所有子类中就可以使用了），只有一些很具体的方法在具体的某个组件中比如Activity是与页面有关的所以关于窗口的方法在Activity（getWindowsManager()等等类似）里面。
Context作为Android中程序的运行环境是Android系统定制的，所有关于他的子类我们就不要再通过new来生成了，就算你使用new生成了，那样生成的也仅仅是普通对象，而不是Android系统中的组件，他没有被系统赋予生命周期，所以是没有任何价值的。要对Android系统的运行环境有个认识，我们的App是在一个Context环境下运行的，这些组件是由系统定制的，在环境下赋予的生命周期，这一点是和Java不同的。
<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  关注微信公众号，及时获取内容更新
</p>

