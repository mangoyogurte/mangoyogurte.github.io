---
layout: post
title: 移动端web开发那些事
description: "进入移动端开发前你需要知道这些事"
tags: [移动web]
modified: 2017-06-26
image:
  feature: abstract-10.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

最近在学移动端web开发，很多概念、技术学起来模糊不清，特地查资料并梳理对比一下，以供以后再来复习。

### 自适应和响应式

- 自适应是为了解决如何才能在不同大小的设备上呈现同样的网页，但是这样的话会出现问题，如果屏幕太小，即使网页能够根据屏幕大小进行适配，但是会感觉在小屏幕上查看内容过于拥挤。响应式正是为了解决这个问题而衍生出来的概念。响应式是为了解决如何才能在不同大小的设备上呈现内容相同排版却不同的网页。
- 简单来说，自适应改变元素大小，响应式改变元素排版。
- 明白了自适应和响应式的区别后，如何应用就很明显了。我们在做移动端web开发的时候，先做自适应，如果感觉内容过于拥挤，我们再局部使用响应式。（为什么不上来直接使用响应式？因为响应式的开发和维护成本太高，所以我们能不用尽量不用。）

### meta标签中的viewport

- 移动设备上浏览器的可视区域叫viewport，可以通过document.documentElement.clientWidth来获取。
- 我们经常在移动端页面的头部看到这样的代码

```html
<meta name="viewport" content="width=device-width,user-scalable=no,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0">
```

这行简单分析一下这一行代码的含义：width=device-width表示网页的宽度等于设备屏幕的宽度，initial-scale=1.0表示设置页面初始的缩放比例为1，user-scalable=no表示禁止用户进行缩放，maximum-scale=1.0 和 minimum-scale=1.0 表示设置最大的和最小的页面缩放比例。因为各大浏览器对meta标签的解析程度不一样，所以我们要尽可能的去兼容所有浏览器。

### dPR（devicePixelRatio）

- 在移动端浏览器中以及某些桌面浏览器中，window对象有一个devicePixelRatio（设备像素比）属性，它的官方的定义为：设备物理像素和设备独立像素的比例，也就是 devicePixelRatio = 物理像素 / 独立像素。
- 在早先的移动设备中，屏幕像素密度都比较低，如iphone3，它的分辨率为320x480，在iphone3上，dPR=1，一个css像素等于一个屏幕物理像素。后来随着技术的发展，移动设备的屏幕像素密度越来越高，从iphone4开始，苹果公司便推出了所谓的Retina屏，分辨率提高了一倍，dPR=2，变成640x960，但屏幕尺寸却没变化，这就意味着同样大小的屏幕上，像素却多了一倍，这时，一个css像素等于两个物理像素。

## 接下来我们先说说自适应中常用且实用的技术

### rem布局

- rem是CSS3新增的一个相对单位，这个单位与em有什么区别呢？区别在于用rem为元素设定字体大小时，相对的只是HTML根元素。而em是相对于其父元素来设置字体大小的。所以rem更实用。

- rem的计算方式这里就不赘述了。直接说使用rem的好处，由于元素的rem关联到根元素的font-size，所以无论移动端设备是什么分辨率，我们只需要改变根元素的font-size大小，那么所有元素的长宽值都会自动跟着改变，达到自适应的效果。操作上来说，布局的时候我们只需要使用rem为单位，在设备分辨率改变时只需要修改根元素的font-size，是不是十分方便？

- 那么问题来了，根元素的font-size是不是随便怎么写呢？当然不是，我们应当根据设计稿来计算。一般设计稿都是基于iphone4或者iphone5，设计稿横向分辨率为640px，dPR=2，那么font-size=deviceWidth/(设计稿横向分辨率/(50*dPR))。deviceWidth即是viewport的宽。比如开发用的设备的viewport为640px时，font-size=100px，布局的时候1rem=100px。css宽度=切图时量取的宽度/font-size，再模仿设计稿进行实际的布局，即可完成自适应的移动端网页。

- 为了方便，阿里团队提供了高清方案布局代码，所谓高清方案就是根据设备屏幕的DPR动态设置html的font-size,同时根据设备DPR调整页面的缩放值，进而达到高清效果。此方案仅适用于移动端web，代码如下。

```html
<script>!function(e){function t(a){if(i[a])return i[a].exports;var n=i[a]={exports:{},id:a,loaded:!1};return e[a].call(n.exports,n,n.exports,t),n.loaded=!0,n.exports}var i={};return t.m=e,t.c=i,t.p="",t(0)}([function(e,t){"use strict";Object.defineProperty(t,"__esModule",{value:!0});var i=window;t["default"]=i.flex=function(e,t){var a=e||100,n=t||1,r=i.document,o=navigator.userAgent,d=o.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i),l=o.match(/U3\/((\d+|\.){5,})/i),c=l&&parseInt(l[1].split(".").join(""),10)>=80,p=navigator.appVersion.match(/(iphone|ipad|ipod)/gi),s=i.devicePixelRatio||1;p||d&&d[1]>534||c||(s=1);var u=1/s,m=r.querySelector('meta[name="viewport"]');m||(m=r.createElement("meta"),m.setAttribute("name","viewport"),r.head.appendChild(m)),m.setAttribute("content","width=device-width,user-scalable=no,initial-scale="+u+",maximum-scale="+u+",minimum-scale="+u),r.documentElement.style.fontSize=a/2*s*n+"px"},e.exports=t["default"]}]);  flex(100, 1);</script>
```

使用方法：把上面这段已压缩过的代码放到HTML的head标签中即可。

- 注意：绝不是每个地方都要用rem，rem只适用于固定尺寸！在相当数量的布局情境中（比如底部导航元素平分屏幕宽，大尺寸元素），你必须使用百分比或者flex才能完美布局！

### 百分比布局和弹性布局以及Flex

- 百分比布局是最常用的自适应手段，大家都很熟悉，不再赘述。想要说说的是用Flex实现的百分比布局（即弹性布局），代码更加清晰简便。具体用法推荐这篇博客，

[用Flex实现的百分比布局](http://blog.csdn.net/handsome_fan/article/details/70199238)

- Flex布局当然不只是用于百分比布局这么简单了，用于各种排版(垂直居中，斜线)的时候也是相当犀利。具体用法阮一峰老师的博客说的非常详细，

[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

## 接下来是响应式中常用且实用的技术

- 目前一般常见的实现响应式有两种方法，一种是利用媒体查询，另外一种是bootstrap下的栅格布局。
- 先说第一种，第一种有两个应用。

1. 选择加载CSS，比如这种代码

    ```html
    <link rel="stylesheet" type="text/css" media="screen and (max-device-width: 400px)"  href="tinyScreen.css" />  
    ```

2. 同一个CSS文件中，比如这种代码

    ```css
    @media screen and(max-device-width: 400px){
　　　　  .column {  
　　　　　　  float: none;  
　　　　　　  width:auto;  
　　　　  }  
    }
    ```

- 至于后一种，笔者还没学习，以后再补充吧o(╥﹏╥)o。

### 说了那么多，总结一下移动端开发适配中的常用技术以及操作顺序

1. 在HTML的头部加入meta标签
    告诉浏览器网页宽度等于设备屏幕宽度，且不进行缩放。

2. Js动态设置根元素的font-size属性

3. 页面使用rem布局

4. 巧用百分比布局和flex
    在页面布局中，相对宽度和绝对宽度（rem即是固定尺寸，即是绝对宽度）相结合来进行布局。

5. 如果有需要，使用响应式中的媒体查询技术进行局部调整元素大小或者改变排版。

### 最后附上移动端开发的协作流程

1. 视觉设计阶段，设计师按宽度750px（iPhone 6）做设计稿，除图片外所有设计元素用矢量路径来做。设计定稿后在750px的设计稿上做标注，输出标注图。同时等比放大1.5倍生成宽度1125px的设计稿，在1125px的稿子里切图。

2. 输出两个交付物给开发工程师：一个是程序用到的@3x切图资源，另一个是宽度750px的设计标注图。

3. 开发工程师拿到750px标注图和@3x切图资源，完成iPhone 6（375pt）的界面开发。此阶段不能用固定宽度的方式开发界面，得用自动布局（auto layout），方便后续适配到其它尺寸。

4. 适配调试阶段，基于iPhone 6的界面效果，分别向上向下调试iPhone 6 plus（414pt）和iPhone 5S及以下（320pt）的界面效果。由此完成大中小三屏适配。

注意第三步，就要使用我们以上介绍的rem布局（rem布局是根基，其他的自适应或响应式技术根据需要来补充使用）。解释一下为什么要在@3x的图里切，这是因为现在市面上也有不少像魅蓝note这种超高清屏幕，devicePixelRatio已经达到3了，这个切图保证在所有设备都清晰显示。

此文参考：

[响应式和自适应的区别](http://blog.csdn.net/bboyjoe/article/details/46501977)

[浅谈移动端的自适应问题——响应式、rem/em、利用Js动态实现移动端自适应](http://blog.csdn.net/duzanuolu/article/details/63135402)

[移动前端开发之viewport的深入理解](http://www.cnblogs.com/2050/p/3877280.html)

[手机端页面自适应解决方案—rem布局进阶版](http://www.jianshu.com/p/985d26b40199)

[从网易与淘宝的font-size思考前端设计稿与工作流](http://www.cnblogs.com/lyzg/p/4877277.html?utm_source=caibaojian.com)