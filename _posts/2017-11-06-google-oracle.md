---
layout: post
title:  "Google和Oracle的官司"
date:   2017-11-06 11:05:54
categories: 程序员要懂的小知识
tags: Google、Oracle
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}















> 文章最早发布于我的微信公众号  Android_De_Home 中，欢迎大家扫描下面二维码关注微信公众获取更多知识内容。            
本文为sydMobile原创文章，可以随意转载，但请务必注明出处！


Google和Oracle的官司一直以来是Android开发程序员津津乐道的。那么就来简单叙述一下，这场官司产生的原因。    

Java不仅仅是一门编程语言，更是一个平台，当年，Java在Sun手中的时候，Sun就推出了J2ME环境，试图来统一手机市场，以自己的Java虚拟机作为一个通用的移动开发平台，但是没有成功。但这不影响Java的影响力，Java的影响力仍然很大，Sun的Java虚拟机仍在许多著名的手机制造商的手机上使用，比如：诺基亚的Symbian系统，RIM的Blackberry系统都采用了Java虚拟机作为程序的运行环境，不过在Android和iOS的兴起中Java虚拟机在手机上的使用已经变得越来越少了。

Java是门编程语言，我们大家都可以进行使用（开发软件级别），当然Oracle是很希望广大的程序使用Java开进行开发的，但是Oracle毕竟是商业公司，商业公司更关心的是自己的利益，那么利益如何产生和保证呢？那就需要保持一个统一的平台，并且使这个平台在自己的控制下，Java的运行环境所必须的就是Oracle的Java虚拟机，如果软件的厂商想要在自己的手机上面使用Java虚拟机必须要经过Oracle的授权，这就会有利益产生了。（语言本身是不需要收费的，但是整个Java的环境需要授权比如：Java虚拟机，当然对于我们普通开发者来说在自己电脑上开发Java软件是不需要的，但是到了厂商级别就需要了）。   

Oracle目前是Java的所有者，拥有在语言和平台上的专利，Android的发展其实是促使了更多的人来使用Java语言，换句话说是促进了Java的发展，但是Google的Android系统有自己的虚拟机，没有使用Java平台的虚拟机，这样的话Java使用的再多对Oracle也没有任何利益产生了，所以Oracle要起诉Google了。   

对于Java的所有者来说Oracle来说，拥有Java的运行的平台才能保证自己的利益，而不是仅仅使用Java语言，他不希望别人去开发自己的运行平台，而Google恰恰是违反了这一点，Google的Dalvik虚拟机采用了大量Apache下的开源Java运行平台Harmony。Android的Dalvik虚拟机运行的不仅仅是Java程序，可以说Dalvik完全可以运行其他的语言开发的程序，但是Google为了吸引Java程序员，运行程序员使用Android的SDK将Java代码转换成Dalvik可以运行的代码，它是如何实现的呢？Google公司在开发Android的时候，雇佣了Sun的一些程序员，利用Harmony中的开源Java库开实现Java程序的转换，避开了授权费用，这样就意味着开发者可以使用Java语言来为非Java平台开发程序，Android的火爆不能给Oracle公司带来任何的商业利用，而且还造成了平台的分裂。  

自从Oracle掌管Java之后，Apache的Java开源项目Harmony也被Oracle拒之门外，（Harmony 是Apache软件基金会主导的开放源代码项目，是自由Java实现计划的一部分，基于Java SE5与6，目标是以开放源代码方式，实现出Java SDK。J2SE 5.0完整性99%,J2SE6.0完成 97%。Android操作系统历来是Harmony的用户，在Android Nougat后依赖OpenJDK库，将专利的JDK替换为开源方案的OpenJDK，以彻底解决ava的专利问题），Oracle拒绝给Harmony提供兼容性测试，这样就是意味着Harmony与Java平台的分裂，这也表明，Java的版本在Oracle这里，用Java语言就要受限于Oracle。  
当开源的Java运行平台Harmony在Apache协议下出现的时候，Sun公司就不高兴，当时没有出现大的冲突可能是因为这个平台还不成气候吧。

<br />
<br />
<p align="center">
<img alt="AndroidInterviewQuestions" src="https://raw.githubusercontent.com/sydmobile/sydmobile.github.io/master/pic/myqr.png"><br />
  关注微信公众号，实现碎片化学习
</p>

