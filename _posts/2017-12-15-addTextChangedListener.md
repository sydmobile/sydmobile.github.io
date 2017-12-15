---
layout: post
title:  "addTextChangedListener简单介绍"
date:   2017-12-15 09:51:54
categories: Android
tags: TextView EditText
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}










>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！  


### addTextChangedListener(TextWatcher watcher)   
![api说明](http://img.blog.csdn.net/20171214175749940?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)     

首先这个方法是属于TextView这个类的，EditText继承自TextView，所以我们可以在EditText中使用这个方法。   
给这个控件增加一个TextWatcher对象，就是监听者，这个监听者负责监听TextView的内容是否发生变化，当发生变化的时候就会调用TextWatcher里面的方法。   
在Android 1.0 API中如果你调用setText方法，TextWatcher里面的afterTextChanged方法是不会被调用的。但是现在的版本这个bug已经改正了。    

### TextWatcher里面的方法
![TextWatcher方法说明](http://img.blog.csdn.net/20171214175844474?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)      

TextWatcher接口中主要有三个方法：beforeTextChanged、onTextChanged、afterTextChanged   
当TextView（以及他的子类）中的文本发生改变的时候，就会用到这里面的方法。  

beforeTextChanged(CharSequence s,int start,int count,int after);   
这个方法是在内容要改变之前调用的，分别解释方法里面的参数   
s: 是在你刚刚输入内容之前TextView里面的内容  
start：是你输入的内容要开始的地方    
count：是你输入的内容要替代的字符（空字符为0），在输入的时候count的值是0，删除的时候count是1   
after：是你新输入的内容的字符数，删除的时候值是1    
综合表达一下就是：从start位置开始，count个字符（空字符是0）将被after个字符替换   


onTextChanged(CharSequence s,int start,int before,int count)  
这个方法是在内容改变后调用的，同样分别解释方法里面的参数   
s:是你输入内容后的TextView里面的内容   
start：是你输入的内容要开始的地方   
count：是你要输入的内容的字符数   
before：是你输入之前在你输入内容开始的地方的字符数   
综合表达一下就是： 从start位置开始的count个字符替换了之前的 before个字符。    

这么说可能有点不明白，举个栗子：   
比如：EditText里面原来是 "weee"这几个字符，则当你输入新字符的时候，这个监听就会被触发，比如你输入 “w”，beforeTextChanged()里面的参数值为：start的值为4（要开始的地方是4），count的值为0（开始的地方是没有字符的，所以值为0），after的值为1（要添加的字符没1个），s的值为 “weee”    
onTextChanged（）里面的参数值：  
s: weeew   
start:4（要开始的地方是4）   
before:(输入之前这个位置是0个字符，值就是0)   
count（输入的字符个数，w是一个字符，就是1） 
<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  更多知识内容
</p>
