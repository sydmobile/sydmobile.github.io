---
layout: post
title:  "从原理上说说ScrollView嵌套ListView的问题"
date:   2018-03-19 15:14:54
categories: Android
tags: ScrollView ListView 
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}










>文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。          
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！ 

ScrollView嵌套ListView会出现的问题，相信大家已经见到的非常多了，对于解决方法也是了如指掌了。但是原理你清楚了吗？这里主要讲为什么会出现这种问题，已经解决这个问题的原理。        

#### ScrollView嵌套ListView出现的问题

ScrollView嵌套ListView会出现的问题，相信大家都已经见的非常多了，对于怎么解决也不陌生了。             

这里再来说一下：  
**出现的问题：**    
ListView会显示不全     

**解决方法：**     
最常见的一种方法：    

自己继承ListView，重写onMeasure()方法    

``` Java
	@Override
	public void onMeasure(int widthMeasureSpec,int heightMeasureSpec){
		super.onMeasure(widthMeasureSpec,MeasureSpec.makeMeasureSpec
		(Integer.MAX_VALUe)>>2,MeasureSpec.AT_MOST));

	}
```
一般的我们仅仅需要这样重写这个方法就可以很顺利的解决ScrollView嵌套ListView出现的ListView显示不全的问题了。那么是因为什么呢？下面我们就从原理上说说！  

#### 解决原理 

说起原理就可MeasureSpec类分不开了，先来介绍一下这个类。  

![MeasureSpec类](//img-blog.csdn.net/20180313153947523?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

MeasureSpec类是View的一个内部静态类，MeasureSpec类封装了从父布局到子布局传递的布局需求。每个MeasureSpec对象代表了宽度和高度的要求。一个MeasureSpec类的表示由控件大小和模式两组成。有三种模式：  

- UNSPECIFIED  
父布局没有对子布局施加任何的限制，子布局可以是任何他想要的大小    
- EXACTLY  
父布局已经确定了子布局的大小，子布局会在父布局给出的界限内。子布局的大小是精确的。父布局给多大就是多大。  
- AT_MOST   
父布局会给定一个最大的值，子布局的大小是不能超过这个值的。但是可以比这个值小。  

MeasureSpec类为了减少对象的分配用了一个整数来实现这个功能（父布局传递到子布局的的布局需求），这个整数是用模式和大小来表示。   

那么这个整数是怎么来实现这个功能的呢？  

![int表示形式](//img-blog.csdn.net/20180313164012230?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

我们都知道int类型的是32位，那么表示形式就是，向上面图中的那样，前两位代表了模式（就是前面提到的那三种），后30位代表了组件的大小。这样就用整数形式来表示模式和大小了。   

#### 具体的看一下三种模式
UNSPECIFIED 模式  
UNSPECIFIED == 0 << MODE_SHIFT  也就是 0 向左位移30位，结果就是int类型的最高位是 00   


EXACTLY 模式    
EXACTLY == 1 << MODE_SHIFT ；也就是 01向左位移30位，结果就是int类型的最高两位是 01   

AT_MOST  模式
AT_MOST = 2 << MODE_SHIFT ；也就是 10 向左位移30位，结果就是int类型的最高两位是 10

#### makeMeasureSpec（）方法 

在这个MeasureSpec类中最重要的一个方法恐怕就是makeMeasureSpec这个方法了。 
![makeMeasureSpec](//img-blog.csdn.net/20180313175232661?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N5ZE1vYmlsZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这个方法就是用给定的大小和模式创建一个int类型的数来满足父布局到子布局传递的布局需求。第一个参数 size就是父布局给子布局传递的大小，第二个参数是模式（就是在上面的三个模式中选择一个）。好了，到这里makeMeasureSpec（）这个方法也讲了。

#### ScrollView嵌套ListView解决方法 

方法一：     

上面已经讲了，重写ListView的onMeasure（）方法   

``` Java
	
	@Override
	protected void onMeasure(int widthMeasureSpec,int heightMeasureSpec){
			super.onMeasure(widthMeasureSpec,MeasureSpec.makeMeasureSpec(Integer

			.MAX_VALUE >> 2,MeasureSpec.AT_MOST));
	}
	
```
我们在修改的时候并没有改变widthMeasureSpec，仅仅是修改了heightMeasureSpec，因为ScrollView设置成了上下滑动，横向并没有滑动，所有在横向上并没有和ListView产生冲突。所以传入的widthMeasureSpec是正确的，而heightMeasureSpec是不正确的，因为ListView嵌套在ScrollView中，也就是说ScrollView是ListView的子布局，这个时候他们的滑动事件发生了冲突，这个值也就不正确了，不是LIstView的实际高度。所以我们要重写传入height，第一个参数为什么是Integer.MAX_VALUE >>2 呢 ？我们说了MeasureSpec用 int类型表示前两位代表模式，后30位代表大小，我们就需要让后面30位是int类型中最大的值就可以了。为什么选择AT_MOST模式呢？这个模式是父布局给定一个值，不能超过这个值，我们很显然已经给了最大值了。 

方法二：  

既然测不出高度，那么我就手中在代码中设置ListView的高度。  

``` Java
	public static void setListViewHeightBasedOnChildren(ListView listView) {
	    if(listView == null) return;

	    ListAdapter listAdapter = listView.getAdapter();
	    if (listAdapter == null) {
	        // pre-condition
	        return;
	    }

	    int totalHeight = 0;
	    for (int i = 0; i < listAdapter.getCount(); i++) {
	        View listItem = listAdapter.getView(i, null, listView);
	        listItem.measure(0, 0);
	        totalHeight += listItem.getMeasuredHeight();
	    }

	    ViewGroup.LayoutParams params = listView.getLayoutParams();
	    params.height = totalHeight + (listView.getDividerHeight() * (listAdapter.getCount() - 1));
	    listView.setLayoutParams(params);
}
```
这种方法有前提条件限制：  
Adapter中getView方法返回的View的必须由LinearLayout组成，因为只有LinearLayout才有measure()方法，如果使用其他的布局如RelativeLayout，在调用listItem.measure(0, 0)；时就会抛异常，因为除LinearLayout外的其他布局的这个方法就是直接抛异常！  

**总结** 自定义ListView比较好用，还有一个问题就是无论使用上面哪一个方法，当你的ListView在加载数据的时候，如果当前页面没展示完全，那么scrollView会自动往下滑动一点，也就是造成了你进入这个页面的时候，默认页面是往下滑动了一下，而不是在最顶端。  造成这个问题主要的原因还是焦点问题，ListView默认情况下，isFocusableInTouchMode和isFocusable都是false的，但是当在加载数据后这两个值就会变为true了。如果在布局中没有其他view获取焦点，这个时候ListView就争夺到了焦点，也就造成了滑动。

这个问题的解决方法：  
1.在加载完数据后设置ScrollView滑动到顶部scrollView.smoothScrollTo(0,0)  
这种做法是有缺点的，你会看到屏幕滑动一下。   

2.使用descendantFousability属性  

descendantFocusability有三种属性  
beforeDescendant：viewgroup会优先其子类控件而获取到焦点。   
afterDescendant：viewgroup只有当其子类控件不需要获取焦点的时候才获取焦点。  
blocksDescendants: viewgroup会覆盖子类控件而直接获得焦点  

在ScrollView的LinearLayout中添加android：denscendantFocusability = "blocksDescendants"就可以了。   

![欢迎大家关注我的微信公众号，和我交流分享](http://img.blog.csdn.net/20180301210553345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3lkTW9iaWxl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
