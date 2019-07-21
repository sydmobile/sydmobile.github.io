---
layout: post
title:  "从0系统学Android-1.3创建你的第一个 Android 项目"
date:   2019-07-21 15:14:54
categories: 从0系统学Android
tags: Android 关键字
excerpt: 预览内容
mathjax: true
author: sydMobile
---
* content
{:toc}














![f](https://github.com/sydmobile/sydmobile.github.io/blob/master/pic/%E5%A4%B4%E5%9B%BE%E7%89%87%E4%B8%A2%E5%A4%B1.png?raw=true)

### 1.3 创建你的第一个 Android 项目

环境搭建完成后，我们就可以写下我们的第一个项目了。

#### 1.3.1 创建 HelloWorld 项目

在 Android Studio 的欢迎页面点击 `Start a new Android Studio project` 就会自动为我们创建一个项目。（首次开启项目，可能构建时间很长，需要下载很多东西，和你的网速有关系）

#### 1.3.2 启动模拟器

我们还可以通过 Android Studio 来创建一个模拟器，供我们运行程序。不过建议使用真机测试。

#### 1.3.3 运行程序

手机和 Android Studio 连接上后，我们就往手机上面运行程序了。

#### 1.3.4 分析你的第一个 Android 程序

1. gradle 和 .idea

   这两个目录下放置的都是 Android Studio 自动生成的一些文件，我们无需关心。也不要去手动编辑

2. app

   项目中的代码、资源等内容几乎都放在这个目录下。

3. build

   无需关心，编译产生的文件

4. gradle

   这个目录下包含了 gradle wrapper 的配置文件，使用 gradle wrapper 的方式不需要提前将 gradle 下载好，而是会根据本地的缓存情况决定是否需要联网下载。

5. .gitignore

   版本控制有关

6. build.gradle

   全局的 gradle 构建脚本。

7. gradle.properties

   全局的 gradle 配置文件。在这里配置的属性会影响到项目中所有的 gradle 编译脚本。

8. gradlew 和 gradlew.bat

   用来在命令界面中执行 gradle 命令的，其中 gradlew 是在 Linux 或者 Mac 系统中使用的，gradlew.bat 是在 Windows 系统中使用

9. HelloWorld.iml 

   是所有的 IntelliJ IDEA 项目都会自动生成一个文件，用于标识这是一个 IntelliJ IDEA 项目。

10. local.properties

    指定本机中的 SDK 路径

11. setting.gradle

    指定项目中所引入的模块。

除了 APP 目录以外，大多的文件和目录都是自动生成的，不需要我们去修改。app 目录才是我们关注的重点。

**APP 目录下进行分析**

1. build

   编译时自动生成的文件

2. libs

   使用了第三方 jar ,存放目录

3. androidTest

   编写 Android Test 测试用例的，可以对项目进行一些自动化测试

4. Java 

   放置代码的地方

5. res

   存放资源，这里面又有很多目录，后面详解介绍

6. AndroidMainfest.xml

   Android 项目的配置文件。我们所使用的四大组件都需要在这里注册，权限的申请也在这里，经常使用

7. test

   编写 Unit Test 测试用例，是对项目进行自动化测试的另一种方式

8. .gitignore

   版本控制(app 模块内)，用于设备版本控制的时候忽略的内容

9. app.iml

   IntelliJ IEDA 项目自动生成的文件

10. build.gradle

    app 模块的 gradle 构建脚本。项目构建相关的配置

11. proguard-rules.pro

    混淆规则。作用：防止我们编译的 apk 包被别人反编译后可以轻松查看。

介绍完这些目录，是不是感觉到很混乱呢，感觉都不知道在说什么，没关系，这些东西后面慢慢接触就清楚了。

**介绍 HelloWorld 项目是如何启动的**

首先查看 `清单文件 AndroidManifest.xml`

```xml
<activity android:name = ".HelloWorldActivity">
	<intent-filter>
     <action android:name = "android.intent.action.MAIN"/>
     <category android:name = "android.intent.category.LAUNCHER"/>   
    </intent-filter>
</activity>
```

这段代码就表示对 `HelloWorldActiviy` 进行注册，如果不注册的话是会报错的。

其中

```xml
<action android：name="android.intent.action.MAIN"/>
<category android:name="android.intent.category.LAUNCHER"/>
```

表示这个项目的主 Activity 就是这个，启动程序的时候会首先打开这个 Activity。

Activity 就是我们所看到的页面，下面看一下代码是怎么写的

```java
public class HelloWorldActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hello_world_layout);
    }
}
```

HelloWorldActiviyt 是继承 APPCompatActivity 的，这是一种向下兼容的 Activity，这样可以使用 Activity 在不同系统版本中增加的新特性和功能可以在比较旧的系统上仍然使用（兼容到 Android 2.1）。

Android 程序设计讲究逻辑和视图的分离。界面是不在 Activity 中直接编写的。而是在布局文件中编写界面。

#### 1.3.5 详解项目中的资源

res 目录介绍

- drawable

  用来存放图片

- mipmap

  存放应用图标

- values

  string、color、样式等配置

- layout

  存放布局

有这么多的 mipmap 开头的目录是为了让程序更好的兼容各种分辨率的手机。drawable 文件夹也应该是相同的道理，我们应该自己创建多个目录：`drawable-hdpi` `drawable-xhdpi` `drawable-xxhdpi` `drawable-xxxhdpi` 图片最好分别制定多个，分别放到不同的目录下，程序运行的时候会自动到对应的目录查找。

只有一套图的时候，把图片放到 drawable-xxhdpi 文件夹



#### 1.3.6 详解 build.gradle 文件

Android Studio 是基于 Gradle 来构建项目的，Gradle 是一种非常先进的构建工具。它基于 Groovy 的领域特定语言（DSL）。摈弃了传统的基于 xml （如 Ant、Maven）的各种繁琐的配置。

**注意：在新的版本中 compile 已经被 implementation 代替了，由于是在《第一行代码》参考下，所以下面还是 compile**

```groovy
buildscript{
  repositories{
    jcenter()
  }
  
  dependencies{  
    classpath 'com.android.tools.build:gradle:2.2.0'
  }
  
}

allprojects{
  repositories{
    jcenter()
  }
}
```

jcenter 是一个代码托管库，很多的开源代码都放在这个库里面，声明了这个配置，我们就可以轻松的引用库里面的开源代码了。

dependencies 闭包中使用 classpath 声明了一个 Gradle 插件，之所声明这个插件是因为，Gradle 并不是专门为 Android 项目开发的，Java 、C++ 等很多项目同样可以使用 Gradle 来构建，如果我们想要用 Gradle 来构建 Android 项目就需要我们使用 Gradle 针对 Android 的插件工具了。

通常情况下我们不需要修改这里面的内容，除非我们想要添加一些全局的项目构建配置。

内层 APP 目录下的 build.gradle

```groovy
apply plugin: `com.android.application`
android{
    compileSdkVersion 24
    buildToolsVersion "24.0.2"
    defaultConfig{
        applicationId "com.example.helloworld"
        minSdkVersion 15
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
    }
    buildTypes{
        release{
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'),'proguard-rules.pro'
        }
    }
    
}

dependecies{
    compile fileTree(dir:'libs',include:['*.jar'])
    compile 'com.android.support:appcompat-v7:24.2.1'
    testCompile 'junit:junit:4.12'
}
```

第一行应用了一个插件，一般有两种值可以选择：`com.android.application` 表示这是一个应用程序模块，可以直接运行。`com.android.library` 表示这是一个库模块，只能依附于别的应用程序运行。

下面是一个 android 大闭包，配置项目构建的各种属性。

compileSdkVersion 指定编译版本，这里指定的 24 表示使用 Android 7.0 系统的 SDK 编译。

buildToolsVersion 用于指定项目构建工具的版本

然后 android 包中又嵌套了一个 **defaultConfig 闭包，对项目中的更多细节进行配置。**

applicationId 用于指定项目的包名

minSdkVersion 指定项目最低兼容的 Android 版本

targetSdkVersion 表示你在该目标版本上已经做过充分的测试，系统会启用这个版本的新的特性和功能。

versionCode 指定项目的版本号

versionName 指定项目的版本名

**下面就是 buildType 闭包**，这里面的配置主要是生成安装文件相关的配置，通常只有两个子闭包，一个是 `debug`，一个是`release` 。debug 闭包用于配置测试包。release 用于配置正式包。debug 包可以忽略不写。

查看 `release` 包中的内容：`minifyEnabled` 用于指定是否对项目的代码进行混淆，true 表示是，false 表示否。`proguardFiles` 用于指定混淆使用的规则文件，这里指定了两个文件，一个是 proguard-android.txt ，这个在 Android SDK 下面，是所有项目通用的混淆规则，第二个是 proguard-rules.pro 在当前根目录下，里面编写当前项目的混淆规则，通过 Android Studio 直接运行的都是测试安装文件。

**dependencies闭包**

这里面主要说明当前项目的依赖关系。Android Studio 项目一共有三种依赖关系：**本地依赖、库依赖、远程依赖** 

**本地依赖：** 就是对本地 jar 包或者目录添加依赖关系。

**库依赖：** 对项目中的库模块进行依赖

**远程依赖：** 对远程仓库上面的开源项目进行依赖。

compile fileTree 就是对一个本地依赖声明。它表示将 libs 目录下的所有 .jar 后缀的文件全部添加到项目的构建路径中。

下面的 compile 'com.android.****'  就依赖的远程仓库。添加上这句后，Gradle 在构建项目的时候会首先检查一下本地是否有这个库的缓存，没有就去对应的仓库下载。

库依赖的基本格式是：compile project 加上要依赖的库名称。例如有个库模块叫：helper，则:

```gr
compile project(':helper')
```

testCompile 这个是用于声明测试用例库的。