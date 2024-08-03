---
title: "chrome无法打开网页源码，F12被禁用无法打开开发者工具，开发者工具异常中断"
date: 2024-08-03T15:30:17+08:00
lastmod: 2024-08-03T15:30:17+08:00
draft: false
author: ev01ing
keepStatic: false
auto: true
categories: ["其他"]
tags: ["开发者工具"]
---


最近写一个网站爬虫的时候，在chrome里F12按了后毫无反应，不弹出开发者工具，右键也不显示chrome中右键的内容。用新标签页打开开发者工具后，访问网站直接停止，开发者工具显示`Paused in debugger`信息。


### 无法打开开发者工具和访问源码


应对无法打开开发者工具和网站源码，有以下几种解决方法：
1. 对打开的网站，可以鼠标点进地址栏，然后再按`F12`打开开发者工具，同样按`ctrl + u`就能打开源码模式
2. 输入`view-source:http://xxxx`的方式打开源码模式
3. 如上面所述，先在空白标签页打开开发者工具，然后再输入要打开的网站



### paused in debugger


`Paused in debugger`。是开发者工具的一个行为，默认是打开了`pause on exception`选项，打开的网站抛出了一个exception导致debugger停止了，只需要将此选项关闭即可解决问题。


1. 在开发者工具中点击到`Sources`标签页，一般来说，报`Paused in debugger`打开的就是`Sources`标签页。

2. 关闭开发者工具的`pause on exception`。 如下图所示，点击1所在的框，**将效果点击成截图的样子**，如果已经是截图的样子就跳过。然后点击2所在的框，**变成暂停模式，点击完成后会变成灰色的暂停样式，而不是现在的蓝色播放样式**。

{{< figure src="one.png" >}}

然后，就可以继续开心的爬虫之旅了。





