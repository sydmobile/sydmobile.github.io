---
layout: post
title:  "杜绝APP CRASH"
date:   2017-12-25 08:14:54
categories: Android
tags: android crash
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}






>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
可以随意转载，但请务必注明出处！


## 为什么出现这种方法
当APP在线程中跑出了异常就会导致APP crash。比如我们最常见的NullPointerException空指针异常。有些时候我们不希望这种异常导致我们的APP crash，尤其是在debug状态下，程序很大的时候，编译运行一次也不容易，debug的时候好不容易程序启动起来了，发生了crash就不能debug执行了，有时候会很耽误开发。所有有了这个自定义的异常处理。它可以捕获你的异常，使程序不会crash，这样就可以继续调试了。    

## 具体操作介绍
### 首先介绍Thread.UncaughtExceptionHandler

![UncaughtExceptionHandler文档](http://img.blog.csdn.net/20171225093026489?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


文档说明：当一个线程突然的因为没有捕获到的异常而停止的时候会由这个接口来处理。当一个线程由于没有捕获的异常而终止的时候,Java虚拟机将查询这个线程通过UncaughtExceptionHandler的getUncaughtExceptionHandler。并将调用uncaughtExcetion方法，并且把这个线程和异常作为参数传递过去。如果一个线程没有他的UncaughtExceptionHandler设置， 那么它的ThreadGroup对象作为它的UncaughtExceptionHandler。如果ThreadGroup对象没有特别的处理异常要求，它可以调用getDefaultUncaughtExceptionHandler。即系统默认的的
UncaughtExceptionHandler。  
     
简单来说UncaughtExceptionHandler就是用于在线程中当一些系统没有捕获的异常发生的时候来处理这些异常的。你可以使用系统默认的处理方式，你也可以通过Thread.setDefaultUncaughtExceptionHandler()方法设置你自己定义的异常处理。   

![uncaughtException_method](http://img.blog.csdn.net/20171225093518436?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

注意Thread.setDefaultUncaughtExceptionHandler(CustomUncaughtExceptionHandler)后,只能保证当在你的程序中如果crash没有发生在UI线程（主线程）中而是在别的线程中的时候，这个时候APP是不会出现崩溃的现象的。如果在主线程中出现crash后，APP还是会崩溃的。    
### 进一步防止程序出现Crash

开头已经说了，有很多时候虽然我们的APP会因为各种问题闪退，但是在更多的时候我们是不希望，我的APP闪退的这就出现了下面的方法。  

首先说明这种方法在Activity初始化的时候可能会导致你的APP出现类似ANR的情况（其实并不是ANR，只是状态看起来像，造成的原因是因为Activity还没有完成初始化，也就是生命周期还没有执行完毕就遇到异常了，导致了页面没法显示，所以在正式发布的APP中还是要慎重使用）

如何使用呢？需要和你的后台商量好，在程序中做好标志控制该不该使用ExceptionHandler来处理。如果你的程序某个地方出现大量crash的时候，而这个功能是在Activity初始化后（可能是由于点击某个按钮触动的问题）这个时候你就可以用ExceptionHandler来处理了，让用户在点击这个按钮后，不至于程序崩溃掉。    

核心代码：   
``` Java 
	 new Handler(Looper.getMainLooper()).post(new Runnable() {
            @Override
            public void run() {

                while (true) {
                    try {
                        Looper.loop();
                    } catch (Throwable throwable) {
                       
                            
								               
                    }
                }
            }
        });
```  
通过Handler向主线程的queue中添加一个Runnabel(此处new Handler()里面传参数Looer.getMainLooper()就是为了是向主线程的queue中添加，如果不传这个参数就是默认的了)当主线程执行到我们发送的这个Runnable的时候就会进入我们的while死循环，如果while内部是空的话就会造成代码卡死在这里导致ANR，但是我们在while中调用了Looper.loop(),这就使得我们的主线程又开始工作了，不断的从queue中获取Message执行(其实Android的机制就是这样的不断的从主线程的queue中获取message并且执行)。这样就可以保证以后主线程的所有异常都会从我们手动调用的Looper.loop()处抛出了。   

下面分析一下为什么这样就可以保证主线程的所有异常都会从我们手动调用的Looper.loop()处抛出。
       
![ActivityThread_main()](http://img.blog.csdn.net/20171225094640314?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)       

首先看主线程的源码，我的主线程都是在main()函数里面开始执行的，就像Java一样，这里是入口。我们可以看到在main函数中调用了Looper.loop()。    

![Looper.loop()](http://img.blog.csdn.net/20171225094724797?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)     


Looper.loop()方法的源码，在这里就很明显了，我们可以看到在这个方法中有一个for(;;)死循环,不断的从queue中获取message执行。所以我们的方法就变成了      

```Java
	  for(;;){
            Message msg = queue.next();
            //如果msg是我们post过来的Runnable就会执行下面的代码了
            while(true){
                try{
                    Looper.loop();(的本质就是 for(;;){.....})
                }catch(....){
                    ........

                }

            }

        }
```
      
所以当出现异常的时候就会抛出了。所以这样做的话就可以保证所有主线程中的异常可以被捕获了，单纯这样做的话，子线程中如果出现问题的话还是会crash的，所以就要加上Thread.setDefaultUncaughtExceptionHandler()了。    

原理基本就介绍完了，结合写的代码(代码里面有详细的使用注释)看会更清晰一些。代码地址和使用方法点击  [这里](https://github.com/sydmobile/myAndroidLearn/tree/master/MyStudyDemo/app/src/main/java/com/syd/mystudydemo/exceptionhandler)        

> 参考：https://github.com/android-notes/Cockroach ，谢谢！    

<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  扫一扫关注微信公众号，获取更多干货和资源。
</p>


