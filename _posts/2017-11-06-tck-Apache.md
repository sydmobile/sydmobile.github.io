---
layout: post
title:  "程序员需要知道小知识"
date:   2017-11-06 15:14:54
categories: 程序员要了解的小知识
tags: TCK纠纷 Apache项目机制
excerpt: TCK纠纷 Apache项目机制
mathjax: true
author: sydMobile
---
* content
{:toc}







### TCK纷争
所谓的TCP纷争是这样的：      
Apache Harmony项目是Apache软件基金会主导的开放源代码项目，是自由Java实现计划的一部分，基于Java SE5与6，目标是以开放源代码方式，实现出Java SDK。J2SE 5.0完整性99%,J2SE6.0完成 97%。但是如果想称自己兼容Sun公司实现的JDK，必须要通过JCP（Java Community Process）对其用有的TCK（Technology Compatibility Kit）的测试，Apache Harmony项目一直在努力争取获取JCP的授权，但是由于Sun公司的态度，JCP并没有给Harmony授权TCK许可，而且SUN发布OpenJDK后，还规定了只有派生自OpenJDK的采用GPL协议的开源实现才可以运行OpenJDK的TCK。        
但是Apache的Harmony是Apache协议，与GPLv2协议是不兼容的，Apache董事会和Harmony项目工作人员反对这种带有条件的授权，认为这在开源社区是不可以接受的，所以谈判破裂，有了TCK纷争。    

### Apache项目管理机制
Apache顶级项目（Top Level Project）：每个顶级项目都有独立的委员会来管理，这些顶级的项目又包含一些自项目。    
目前所有的Apache项目都是需要经过孵化器孵化，只有满足了Apache的一系列的质量要求后才能毕业，从孵化器毕业的项目，要么独立成为顶级项目，要么成为其他顶级项目的子项目。随着子项目的不断发展壮大，也有可能从所属的顶级项目中独立出来，单独作为一个顶级项目存在。
