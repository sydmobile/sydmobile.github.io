---
layout: post
title:  "Gradle、Maven、Ant的介绍"
date:   2017-11-06 16:14:54
categories: 程序员要了解的小知识
tags: Gradle Maven Ant
excerpt: Gradle、Maven、Ant的介绍
mathjax: true
author: sydMobile
---
* content
{:toc}








>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！

### Apache Ant
Apache Ant 是一个将软件编译、测试、部署等步骤联系在一起加以自动化的一个工具，大多用于Java环境中的软件开发。默认情况下，它的buildfile（xml文件）名为build.xml，每一个buildfile含有一个```<project>```和至少一个预设的`<target>`，这些targets包含许多task elements。每个task element有一个用来被参考的id，此id必须是唯一的。build.xml范例    

``` xml
<?xml version="1.0" ?>
<project
    name="Hello World"
    default="execute">

    <target name="init">

        <mkdir dir="build/classes"/>

        <mkdir dir="dist"/>
    </target>

    <target
        name="compile"
        depends="init">

        <javac
            destdir="build/classes"
            srcdir="src"/>
    </target>

    <target
        name="compress"
        depends="compile">

        <jar
            basedir="build/classes"
            destfile="dist/HelloWorld.jar"/>
    </target>

    <target
        name="execute"
        depends="compile">

        <java
            classname="HelloWorld"
            classpath="build/classes"/>
    </target>

</project>
```

### Apache Maven
Apache Maven：是一个软件（特别是Java软件）项目管理以及自动构建工具，由Apache软件基金会所提供。是基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。     
Maven也可以被利用与构建和管理各种项目，例如：C#、Ruby、Scala和其他语言编写的项目。Maven曾经是Jakarta项目的子项目，现在已经成为Apache软件基金会主持的独立Apache项目了。  
Maven项目使用项目对象模型（Project Object Modle，POM）来配置项目，对象模型存储在名为pom.xm的文件中。   
简单示例：

``` xml
<project>
    <!-- model version is always 4.0.0 for Maven 2.x POMs -->
    <modelVersion>4.0.0</modelVersion>

    <!-- project coordinates, i.e. a group of values which
         uniquely identify this project -->

    <groupId>com.mycompany.app</groupId>

    <artifactId>my-app</artifactId>

    <version>1.0</version>

    <!-- library dependencies -->

    <dependencies>

        <dependency>

            <!-- coordinates of the required library -->

            <groupId>junit</groupId>

            <artifactId>junit</artifactId>

            <version>3.8.1</version>

            <!-- this dependency is only used for running and compiling tests -->

            <scope>test</scope>

        </dependency>
    </dependencies>
</project>
```
### Gradle
Gradle 是一个基于Apache Ant和Apache Maven概念的项目自动化构建工具。它使用一种基于Groovy语言的特定领域语言（DSL语言，所谓的DSL是指这个语言应用在特定的领域，而类似Java这样是DCL语言，可以运用在普通的各个领域）来声明的项目设置，而不是传统的xml语言，当前支持的语言仅限于Java、Groovy、Scala、Kotlin。计划未来支持更多的语言。支持maven、lvy仓库。   
过去Java开发者们常用Maven和Ant等工具进行封装部署的自动化，或者两者兼用，不过这两个工具彼此有优缺点，如果频繁改变相依赖包的版本，使用Ant相当麻烦，如果琐碎工作很多，Maven功能不足，而且两者都是使用xm描述的，相当不利于设计if、switch等判断式，即使写了可读性也不佳，Gradle改良了过去Maven、Ant带给开发者的问题，也已经成为Android Studio内置封装部署工具，比如添加项目依赖，发布版本，混淆配置等等。    
优点：    

1. 自动处理包相依赖关系---取自maven repos的概念
2. 自动处理部署问题--取自Ant的概念
3. 条件判断写法直觉--使用Groovy语言

Android Studio中gradle配置文件的介绍：   
``` Groovy
//声明是Android程序
apply plugin: 'com.android.application'
//关于Android项目的一些配置
android {
    compileSdkVersion 24 // 编译版本号（android.jar）
    buildToolsVersion '24.0.0' // build tools的版本号
    //对lint的配置，lint可以用来检查代码
    lintOptions {
        // 打包的时候省去不用的资源
        disable "ResourceType"
        checkReleaseBuilds false
        // 关闭
        abortOnError false
    }
    // 一些默认的配置
    defaultConfig {
        applicationId "com.syd.demo"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 301
        versionName "3.0.1"
        multiDexEnabled true
        ndk {
            //选择要添加的对应cpu类型的.so库。
            abiFilters 'armeabi', 'armeabi-v7a', 'armeabi-v8a'
            // 还可以添加 'x86', 'x86_64', 'mips', 'mips64'
        }
    }
    // 对一些构建的配置
    buildTypes {
	    // 对打包发布的一些配置
        release {
	        // 是否进行混淆
            minifyEnabled false
            // 混淆文件的位置
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        }
    }
    // 签名配置，配置上之后再运行或者debug的时候就会使用你的签名打成包了而不是默认的
    signingConfigs {
        debug {
	        //签名文件的位置（直接和本文件放在一层就好了）
            storeFile file('xxx.keystore')
            storePassword "xxxx"
            keyAlias "xxxx.keystore"
            keyPassword "xxx"
        }
    }

}
// 项目的一些依赖库的添加
dependencies {
	// 编译libs目录下的所以jar包
    compile fileTree(include: ['*.jar'], dir: 'libs')
   
    compile files('libs/universal-image-loader-1.9.5.jar')
    //添加maven仓库里面的依赖
    compile 'com.xxx.xx.xx'
	//项目模块依赖 里面的项目路径是相对于本文件的
    compile project(':项目名字’)
}
// 对依赖的仓库的声明
repositories {
	// 可以理解为一个在网络上的存放依赖包的仓库，在这里声明后，你才可以直接在上面添加这个仓库里面的依赖包，android studio默认的是jcenter仓库里面的
    mavenCentral()
}
```   

proguardFiles这部分有两段，前一段代表系统默认的android程序的混淆文件，该文件已将包含了基本的混淆声明，免去了我们很多的事情，这个文件的目录在你的sdk目录中的tools/proguard/proguard-android.txt 后面的一部分是我们项目里面自己定义的混淆文件，目录就是 app/proguard-rules.txt如果你使用的是Android Studio的话，创建的新的项目的时候默认生成的文件名是 proguard-rules.pro 在这里你可以声明一些第三方依赖的混淆规则，最终混淆的结果是这两个部分所共同决定了，关于混淆的知识在以后的文章里面会发布。  

每一个module都需要一个Gradle配置文件，语法都是一样的，唯一不同的就是开头的声明，项目的开头声明是apply plugin:'com.android.application'  而作为类库依赖的开头是：apply plugin：’com.android.library'   
项目目录下的gradle是整个项目的gradle基础配置文件，里面主要包含了：声明依赖和插件的仓库的来源   

```
buildscript {
    repositories {
        jcenter()//这是构建插件所使用的远程库，后需会有一篇介绍库的含义
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.0.0-alpha1'//这是项目所使用的 Android gradle 版本
    }
}

allprojects {
    repositories {
        jcenter()//这是使用远程库依赖的时候，的远程库
    }
}

```


关于远程依赖库的知识后面的文章会讲到，你也可以关注我的微信号，更快的获取更新！

<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  关注微信公众号，及时获取内容更新
</p>

