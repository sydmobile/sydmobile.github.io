---
layout: post
title:  "Apache Maven、Maven仓库、Jcenter仓库"
date:   2017-11-06 15:14:54
categories: 程序员要了解的小知识
tags: ApacheMaven Maven仓库 JCenter仓库
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}








>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。      
>本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

这几个概念作为许多刚开始学习编程的同学们会有很多迷惑，傻傻分不清，今天就来介绍一下。   
### Apache Maven
其实关于Apache Maven在上一篇文章里面介绍过了[Apache Maven介绍](http://blog.csdn.net/sydMobile/article/details/78458704)   
Apache Maven就是一套软件工程管理和整合的工具，基于工程对象模型（POM）的概念，我们需要知道就是Maven可以管理项目的构建、报告和文档，是一种工具。

### Maven仓库
Maven仓库你可以理解为和Apache Maven没有直接的关系，他就是一个存放各种工程jar文件、library文件、插件或者任何工程项目的仓库，用Maven来构建项目的时候可能也会用到Maven仓库里面的依赖库。    

Maven仓库有三种类型：   

- 本地（local）  
- 中央（central），在android开发中最常用
- 远程（remote）

本地仓库：Maven本地仓库是在计算机上的一个文件夹，它在你第一次运行任何maven命令（这里的maven是构建工具）的时候创建，Maven本地仓库会保存你的工程所有依赖（library jar、plugin jar等）当你运行一次Maven构建工具的时候，Maven会自动下载所有依赖的jar文件到本地仓库中，它避免了每次构建的时候都要引用存放在远程仓库中的依赖。   

mavenCentral：中央仓库，这个仓库是由Maven社区管理，由Sonatype公司提供服务，是Apache Maven、SBT和其他构建系统默认的仓库，并且很容易被Apache Ant、Gradle和其他的构建工具使用，需要通过网络访问，通过：http://search.maven.org/#browse 开发者就可以在里面找到自己所需要的代码库。

![](http://img.blog.csdn.net/20171106174426324?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 https://repo1.maven.org/maven2/ 可以查看项目库的列表

![](http://img.blog.csdn.net/20171106174539903?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

远程仓库：如果我们是library的作者，我们不想把library放到mavenCentral的服务器上面，也可以放在自己定义的特有的maven仓库服务器上，这个时候如果别人想使用我们的library就需要这样了

```
repositories{
            maven { url 'library的仓库的服务器地址'}
}
```
然后在dependencies里面正常添加依赖就可以了。  

### JCenter仓库
查看JCenter仓库内容：http://jcenter.bintray.com/   

![](http://img.blog.csdn.net/20171106174935376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


早期的Android Studio的默认使用的仓库是 MavenCentral，但是由于MavenCentral相比jcenter的一些缺点（开发者上传项目文件不方便，安全方面的原因），换成了jcenter。所以在你用Android Studio创建一个项目的时候， jcenter（）会被在项目的build.gradle自动定义。  

> 参考：
> http://www.jcodecraeer.com/plus/view.php?aid=3097
>  http://blog.csdn.net/lu_xin_/article/details/51134849

<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  扫一扫关注微信公众号，获取更多干货和资源。
</p>
