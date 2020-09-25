---
title: 编程笔记 - Java 多线程
date: 2019-01-20 20:50:14
categories: 折腾
---

最近在学校的 NHS* 里做一个有趣的项目——为学校的“赌场之夜”做两个老虎机，编写过程中遇到了些困难，最终学习了 Java 的多线程相关的知识然后解决了问题。

<!--more-->

故事：我们学校的 NHS 开展了一个 Project，就是要以“赌场”为主题，举办一个 Winter Formal (“冬季舞会”)。我负责做两台老虎机。本来想偷懒，直接上网上找开源的 HTML5 的实现，但我的计算机老师鼓励我自己用 Java 实现，不要用现成的。我都想好怎么编了，编出来之后竟然不 work，本来应该是动画的地方只出现了转圈圈，程序冻住几秒，并没有按照预期播放动画。经过思考之后，学习了多线程的知识，然后解决了问题。就此记录该过程。

------

一开始遇到的问题是这样的：写好了动画的代码，用 Thread.sleep(time) 来实现动画，但是运行的时候却只有鼠标转圈圈，没有动画，但是等几秒后（“动画”完成后），摇奖的结果会显示。估计是运行的时候 Thread 还没有更新 GUI 里面的的内容就 Sleep 了，或者是不是应该叫做被“阻塞”了？此时程序的情况如下图：

![](https://raw.githubusercontent.com/mgrddsj/PicLib/master/One%20Thread.gif)

于是我就上 YouTube 上面找 Java 多线程相关的教程，找到如下这个[视频](https://youtu.be/VYN-CBtPNiM)（26和27集连起来看）。同时发现这个[频道](https://www.youtube.com/user/thenewboston)有挺多编程相关的教程的，Java、Python……什么都有。推荐一下。（需要梯子）

<iframe width="560" height="315" src="https://www.youtube.com/embed/VYN-CBtPNiM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
其实就是在原来 GUI 的 class 之外再加一个 class（把原来的 Spin 相关的内容迁移到新的 class 里面，然后在 GUI class 里面提供 accessor method 以供修改显示的图片）。第二个 class 要在 class name 后面加上 implements Runnable ，同时把要运行的那段代码放在一个叫 run 的 method 里面（ IDE 会自动提示加上 import java.awt.EventQueue; ）。然后在 GUI class 里面加上下面的代码就行了（我的第二个 class 的名字叫做 “spin”）。

```java
		Thread t1 = new Thread(new Spin(board,slot1Res,slot2Res,slot3Res));
		t1.start();
```

改好之后程序的程序的架构大概是这样的：

![](https://raw.githubusercontent.com/mgrddsj/PicLib/master/Two%20Threads.gif)



[^NHS]: National Honors Society

 