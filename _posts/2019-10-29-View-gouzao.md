---
layout: post
title:  "从 View 的四个构造方法说起"
date:   2019-09-27 16:14:54
categories: Android
tags: View构造方法
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}














![](https://user-gold-cdn.xitu.io/2019/8/28/16cd8411a1ff4b2d?w=1144&h=505&f=png&s=1736570)  

![声明](https://user-gold-cdn.xitu.io/2019/5/22/16adf0c4dd3185d5?w=1080&h=237&f=png&s=1025845) 

### View 类的四个构造函数

写过自定义 View 的都知道，View 有四个构造函数，一般大家都知道第一个构造方法是简单的在代码中`new` View 的时候调用的，第二个构造方法使用最广泛，是对应的生成 `xml` 中定义的 View 的时候调用的。剩下的两个构造方法，大家了解的就比较少了。一般在自定义 View 的时候都会不加思索的按照固定的写法。

那么你有没有想探究一下里面的关系呢？

#### 构造方法 View(Context context)

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd841a0005b133?w=1005&h=217&f=png&s=655633)

最简单的构造方法，当在代码中创建一个 View 的时候使用。这种构造方法我们一般不会使用。一般都是在 xml 中定义 view ，很少有直接进行 new view 的，这样写法也不符合规范！

#### 构造方法 View(Context context,@Nullable AttributeSet attrs)

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd84177bda651b?w=1046&h=500&f=png&s=1572140)

这个构造方法是我们最常用的，当我们在 xml 中定义了 View 然后在代码中使用这个 View 的时候，这个 View  就是利用这个构造方法生成的。View 的属性值来自 AttributeSet 的值。

#### 构造方法 View(Context context,@Nullable AttributeSet attrs,int defStyleAttr)

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd84202caf9240?w=1049&h=581&f=png&s=1832052)

这个构造方法就是提供了默认的 `defStyleAttr` 用于指定基本的属性。也就是允许 View 有自己基础的风格。例如：一个 Button 类在调用这个构造函数的时候会给 `defStyleAttr` 赋予一个默认的值 `R.attr.buttonStyle`  这个值包含了 Button 的一些基本的风格（会在 Theme 中给出），比如：最小宽度，最大宽度等等基础风格。当然这些值我们都可以在 `xml` 中通过属性直接改变。

#### 构造方法 View(Context context ,@Nullable AttributeSet attrs,int defStyleAttr,int defStyleRes)

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd8424d6a0cf37?w=1273&h=983&f=png&s=3761239)

这个构造方法对比构造方法三就多了一个参数 `defStyleRes`  这个参数的作用就是再提供一个给 View 提供默认属性的手段。`defStyleRes` 就是把一些我们想要的属性写到一个 Style 里面，然后把这个 style 赋值给 `defStyleRes` 就可以了。

#### 四个构造函数总结

**第一个构造函数**：这个构造函数就是在代码中直接 new view 的时候使用，这样出来的 View 默认是没有任何的属性值，需要后面自己手动 set。

**第二个构造函数**：这个构造函数是在代码中生成对应 `xml` 中定义的 View 使用的。这个时候在 `xml` 中定义的属性值会通过 `AttributeSet` 传递，这样生成的 View 对象是有默认的属性值的。

**第三个构造函数**：这个构造函数就是相对于第二个构造函数，多提供了一种给 View 添加默认属性的方式，通过 `deftStyleAttr` 如果没有默认的值，就用 0 。这样做的好处就是，我们可以默认一个 View 的基础风格。比如可以在 `defSyleAttr` 中设置背景颜色，字体大小等等基础风格，这样我们定义的这个 View 就有了原始风格了，当然如果你在 `xml` 中又设置了背景颜色，等于改变了原始的风格，是最优先于 `xml` 中的属性的。

其实我们使用的很多系统 View 都是通过这种方式来，这里用 `Button` 来举个例子

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd8428b54be229?w=849&h=409&f=jpeg&s=50777)

可以看到 Button 在使用第三个构造函数的时候，传入了 `com.android.internal.R.attr.buttonSyle` 这个属性，这个属性我们在属性定义文件 `attr` 中找到，这个属性的出现就是用来定义 Button 的默认风格的。

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd842e4dde37ea?w=618&h=257&f=jpeg&s=25702)

然后在主题中给 `buttonStyle` 赋值，找到系统 Theme

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd8432a3efda34?w=814&h=256&f=jpeg&s=50204)

可以看到在系统的 Theme 中给 `buttonSyle` 赋值了，指向了 `style/Widget.Holo.Light.Button` 这个风格就指定了 Button 的默认风格，当然我们在定义主题的时候，可以自己定义 `buttonStyle` 这个属性的值。

看一下 `style/Widget.Holo.Light.Button` 的内容

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd84363e9e1d8b?w=694&h=165&f=jpeg&s=25842)

这里定义了 Button 的一些默认的样式。

其他的 View 也都一样，都是这样的一个路子。

**第四个构造函数**：第四个构造函数相对第三个构造函数就多了一个 `defStyleRes`  ，说白了就是多了一种提供 View 默认属性的一种方式。这种方式更加的简单，直接在代码中传入 `R.style.XX` 就可以了。如果没有默认值的话就为 0 。这个参数只有 `defStyleAttr` 为 0 的时候才会生效。

### 关于  AttributeSet

我们在 `xml` 布局文件中，定义 View  的时候，会设定这个 View 很多的属性。AttributeSet 就可以看做是这些属性的集合，包含了属性名和属性值。

举例说明：

第一步：定义 CustomTextView 

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd843ad74eeb8a?w=1434&h=749&f=png&s=3228266)

可以看到，我在第二个构造函数中把 AttributeSet 的 name 和 value 都打印出来了。

第二步：xml 中使用 CustomTextView

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd844110cf0ffd?w=644&h=177&f=png&s=342801)

第三步：运行程序，查看结果

![](https://user-gold-cdn.xitu.io/2019/8/28/16cd84453a583465?w=1093&h=199&f=png&s=653881)

可以看到 xml 中定义的 5 个属性全部打印出来。因此 AttributeSet 对应的就是 xml 布局文件中定义的属性

### 给 View 提供样式的方式

- 直接通过 xml 中的属性

  这种方式是最直接的，体现形式就是直接在布局中设置属性，在  xml 布局文件中使用 style 也属于这种方式

- 通过 `deftStyleAttr` 设置属性

  这种方式主要是用来设置默认属性风格的，使用方式见上面 button，主题中设置不同的类型，View的默认风格会发生改变。

- 通过 `defStyleRes` 设置属性

  这种方式是直接在代码中指定一个默认的 style，和 Context 的主题没有关系

- 在 Theme 中设置属性

  这种方式非常不常见，就是把在布局文件中设置的属性，直接放到 Them 中。

如果同时使用这几种方式给 View 设置了属性那么 View 听谁的呢？

**xml中直接定义 > xml 中 style 定义 >defStyleAttr > defStyleRes > Theme 中直接定义**

文字关注：
![](https://user-gold-cdn.xitu.io/2019/5/22/16adf0cca1c726bb?w=2048&h=1365&f=jpeg&s=1855540)

