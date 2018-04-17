---
layout: post
title:  "Android中的Attribute介绍"
date:   2018-04-17 15:14:54
categories: Android
tags: attr
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}












相信这个词对于Android开发者来说十分熟悉了，那么你对他到底有多了解呢？      

回忆起我刚开始接触Android的时候对这三个词有一些迷惑，有些时候只知道一些基本的使用，总之是有迷惑把。不能说的很清楚！     

今天就来仔细说说这三个词，总的说来 attr、style、theme都是用来表达样式风格的，只是范围不一样。下面我们来具体的一个一个的说明。  

本来写的时候没想多会牵扯这么多内容，因为在写的过程，考虑到很细，想要写的尽可能全面，让“0基础的朋友”也可以看懂，所有写着写着就很长了，建议收藏后慢慢看！耐心看到最后！     

也欢迎大家关注我的公众号：Android开发者家园     
你可以通过公众号添加我为好友，一起交流！    

#### Attr基本概念 
 Attr :单词的意思是属性的意思（但是这里的属性和xml控件中的属性不是一个意思，不要混淆！Attr说是属性只是说的是它的单词的意思是属性），我们是通过Attr文件来定义我们控件中所使用的属性的，这样说可能大家会有一迷惑，那么来举个栗子：     
 
 比如我们在控件中最多使用的 layout_width 属性      
 

 ![layout_width](https://img-blog.csdn.net/20180417163149442?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

![layout_width](https://img-blog.csdn.net/20180417163702281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

 这个属性就是在Attr里面定义的,那么如何来查找这个属性呢？看图片   

![Attribute位置](https://img-blog.csdn.net/20180417163753286?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

![Attribute位置](https://img-blog.csdn.net/20180417163824129?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### View属性
 看到上面的图片，我们可以看到在Android的sdk中给我们建立了一个attr文件，
 这里面就是定义了我们在控件中用到的属性。我们再到Android的SDK给我们定义的 attr 文件中去看看    
 
![图片view](https://img-blog.csdn.net/20180417164030658?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

![view](https://img-blog.csdn.net/20180417164107863?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

![view](https://img-blog.csdn.net/20180417164120371?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

![view](https://img-blog.csdn.net/2018041716413077?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


上面的属性是为view这个控件定义的属性,看注释介绍的很清楚，这是为View和他的子类提供的属性组，也就是说这里面的属性都可以用在view和它的子类中，使用。看到这里面的属性是不是很眼熟啊，这里面的属性是不是我们在写布局的时候都有用到过啊。   

##### TextView属性
再来看看Android SDK为TextView定义的属性组     

![TextView 属性](https://img-blog.csdn.net/20180417164516292?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

等等，我们用到的控件在这里都有对应的属性进行声明。看到这里是不是明白了attr的意思了，attr其实就是一个文件，这里面定义了我们的控件中所使用的属性。    

#### 具体的说一下attr的一些知识  

#####   如何定义attr？ 

我们首先来看看Android的官方属性是如何进行定义了  

![TextView属性定义](https://img-blog.csdn.net/20180417164835258?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

我们看到首先声明了一组属性，取了一个名字叫 TextView，然后在这里面定义了一系列的属性。  

我在这里总结了属性的定义格式：  

``` HTML

 1   <declare-styleable name="TextView">
         2  <attr  name ="属性1" format = "这个属性的取值格式">
            3  <enum name="取值1" value="程序中对应的值"/>
               <enum name="取值1" value="程序中对应的值"/>
               <enum name="取值1" value="程序中对应的值"/>
               <enum name="取值1" value="程序中对应的值"/>
            4  <flag name="取值1" value="程序中对应的值" />
               <flag name="取值2" value="程序中对应的值" />
               <flag name="取值3" value="程序中对应的值" />
    </declare-styleable>
```  


其中3和4是可以省略的， format也是可以省略的（我们之所以自定义属性，一般就是在自定义View中使用，如果省略了format的话只是在布局中我们使用这个属性的时候没有提示，只要在布局中填的属性内容和你的 java 文件的取值对应就没问题，不过还是建议format要定义好，这样更清晰不容易乱）3就是我们提前给这个属性设置了几个值，可以直接在这几个值中取。与4的区别就是：flag可以在布局文件中这样使用  取值1|取值2  也就是说可以取多个值。       
例子：
 
 ![layout_width定义](https://img-blog.csdn.net/20180417165407597?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 
这里就是我们常用的layout_width的定义方式：看到我们可以将layout_width的值设置为fill_parent或者match_parent或wrap_content或者自己填写大小。 

 而textStyle  我们的取值就可以是多个了，就不再多介绍了。
 好了，下面我们可以自己来定义属性了。   

![自定义属性](https://img-blog.csdn.net/20180417165541787?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

重点来介绍format里面的一些值        
fraction：百分数       
例子： <attr name = "xshow" format= "fraction"/>
使用：         
app:xshow = "70%"             
reference : 指定某一资源ID              
例子： <attr  name = "backgroundresourece" format = "reference"/>           
使用 ： app:backgroundresourece = "@drawable/id"            
别的格式基本上就是见名知意，就不再介绍了。      

##### 属性的取值   

在某些情况下，我们可能想让某个属性取另一个属性的值，这样说可能不太好懂。看例子!  

![test_attr](https://img-blog.csdn.net/20180417165801792?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

上图是我自己定义的一个属性，在我的布局中有一个TextView,我想让Text的内容取test_attr的内容，怎么办呢？      

![textView中使用test_attr属性](https://img-blog.csdn.net/20180417165909515?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

没错就是 **?attr/属性名字** 这样就是取 test_attr这个属性的值，如果test_attr是
android里面的attr值呢？那么引入方式就是 ：**?android:attr/属性名字 或者 省去attr**       
**以上的操作都有一个前提：**     

attr的值只有在theme中被赋值才有效(否则这样取值的结果就是程序报错，注意在有些主题中有些属性给了默认值，这个时候引用就没有错，但是如果没有默认值，而你又没有在主题中给定义上那样就会出错了)，也就是说必须在清单文件中的 Application或者 Activity中设置 Theme，并且Theme指向的属性有值才可以引用attr的值，在单独的控件中使用 android：Theme 或者 app：theme添加样式是没有用的！   

##### obtainStyledAttributes详细说明    

在自定义View中获取我们自定义的attr的值，一般大家都会这样写：  


``` 
TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.属性组名称, defStyleAttr, defStyleRes);
a.getXX(R.styleable.XX_xxx);
a.recycle();
```
关于方法obainStyledAttributes()   

![obainStyledAttributes](https://img-blog.csdn.net/20180417170805138?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们可以看到这个方法是存在在Context中的方法，最终调用的是getTheme()里面的方法，所以我们有的时候看到 context.obainStyledAttributes和context.getTheme().obainStyledAttributes是其实一样的。  

下面来仔细说说这个方法：  

 我们可以看到

``` Java
      public final TypedArray obtainStyledAttributes(AttributeSet set, @StyleableRes int[] attrs) {
	        return getTheme().obtainStyledAttributes(set, attrs, 0,0);
	  }  

```
最终是调用的是这个

``` Java
    public TypedArray obtainStyledAttributes(AttributeSet set,@StyleableRes int[] attrs, @AttrRes int defStyleAttr, @StyleRes int defStyleRes) {

			return mThemeImpl.obtainStyledAttributes(this, set, attrs, defStyleAttr, defStyleRes);
	}
```
我们就来说一说这个方法简单来说这个方法就是返回一个TypedArray对象（理解为用来存放属性值的数组），里面的参数   
set:一个和xml中的标签关联的存放属性的集合，int[] 就是我们要在xml中读取的属性。     

defStyleAttr：当前主题中的一个属性，其中包含对为TypedArray提供默认值的样式资源的引用。可以为0以不寻找默认值。    

defStyleRes：是具体的style资源。为TypedArray提供默认值的样式资源的资源标识符，仅当defStyleAttr为0或在主题中找不到时才使用。 可以为0以不寻找默认值。   

这么说起来可能有点迷糊，来一个例子保证你立马领悟！   

首先我在attrs中定义一组属性   

![图片myview1定义属性](https://img-blog.csdn.net/20180417171607301?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

其中的 attr_defStyle 属性名，就是我定义的defStyleAttr用obtainStyledAttributes中作为参数。  

然后自己定义style和Theme  

![Theme](https://img-blog.csdn.net/20180417171654956?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

![图片 stylemy](https://img-blog.csdn.net/20180417171720465?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我在上面的注释中已经写得很清楚了，就不多解释了。  

然后再自定义View  

![图片自定义view](https://img-blog.csdn.net/2018041717181694?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

其中第一个构造方法一般就是Java代码中new 控件的使用，第二个构造方法就是在解析xml中控件生成view的时候调用了。  

可以看到我在 第二个构造方法中是这样写的       
this(context, attrs, R.attr.attr_defStyle);       
其中 R.attr.attr_defStyle 就是定义的 defStyleAttr，在自己定义的Theme中给它附了值（这样就解释了前面说的，它是当前主题中的一个属性，包含了对style（style_attr_defStyleAttr）的引用）  

R.style.style_defStyleRes就很好解释了，就是一个style资源引用。  

再来看看布局页面

![xml布局文件](https://img-blog.csdn.net/20180417172116179?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在布局文件中给它设置了text1属性，和style，我们来看一看程序运行的结果

![默认属性全部全了](https://img-blog.csdn.net/20180417172159738?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


看到这个运行结果就得出结论了，关于属性的取值是由顺序的   

1.优先取在布局中给定的值   
2.在布局中设置的style中的值       
3.从defStyleAttr和defStyleRes中取值，注意如果 defStyleAttr有值，则不再去defStyleResult中的值，就算defStyleAttr有的属性没有赋值。（具体看上面的打印结果）      
4.Theme中设置的属性         

**注意** defStyleAttr的值一定要在Theme中设置才有效果，就拿上面的例子说，如果你没有在Theme中给R.attr.attr_defStyle赋值，而是直接在布局文件中赋值，这样做是没有效果的。   

做了上面的介绍，我们再来看看系统是怎么做的，随便看一个button控件   

![button构造方法](https://img-blog.csdn.net/20180417172400839?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们看到 button 的构造方法的defStyleAttr传的是com.android.internal.R.attr.buttonStyle属性，这个属性我们在系统的attr中找到   

![buttonstyle](https://img-blog.csdn.net/2018041717244718?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这就是系统定义的默认属性buttonStyle

我们再来看看系统Theme是怎么给它附的值   

![buttonTheme](https://img-blog.csdn.net/20180417172543746?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里给了一个指引，指向了一个style  

![buttonstyle1](https://img-blog.csdn.net/20180417172614229?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


所以我们的button就在不同的主题中有了默认的样式。看看系统的定义是不是和我们定义的很相像啊！
现在清楚了吗？建议还是多看看源码。  

原创不易，希望大家能够尊重原创！    

##### 小知识点  

``` XML 

<style name="MyStyle" parent="Widget.Button">
   <item name="background">@drawable/btn_default_holo_light</item>
   <item name="textAppearance">?attr/textAppearanceMediumInverse</item>
   <item name="textColor">@color/primary_text_holo_light</item>
   <item name="minHeight">48dip</item>
   <item name="minWidth">64dip</item>
</style>

```  

其中parent可以理解为 MyStyle 继承自 Widget.Button这种继承一般是继承系统的，而自己继承自己的style则是  

```
<style name = "MyStyle.H">
   <item -----></item>
</style>

```
是的这里使用`` .``  表示H 继承自MyStyle    

好了关于Attr的介绍就到这里，本来想一篇文章全部介绍完，结果写着写着就写多了，主要是考虑到许多基础问题，想要大家都能看的懂就写多了。剩下的style和theme估计篇幅也很大。写起来发现牵扯的知识点太多了，什么都想给大家介绍一下。   


>文章最早发布于我的微信公众号  Android开发者家园 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！  

![欢迎大家关注我的微信公众号，和我交流分享](http://img.blog.csdn.net/20180301210553345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

**有什么问题也可以加我好友交流**

![这里写图片描述](https://img-blog.csdn.net/20180417174038452?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


















