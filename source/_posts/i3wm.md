title: i3wm----平铺式窗口管理器
date: 2015-11-02 22:05:17
tags:
- i3wm
- Ubuntu

categories:
- Ubuntu相关

---

![i3](http://i3wm.org/img/logo.svg)

发现了一个好用的平铺式窗口管理器**i3wm**

大概长这样:
![](http://i3wm.org/screenshots/i3-9.bigthumb.png)

试用了一下感觉还不错,操作方便,配置简单,最重要的是很省资源,这下连XFCE的桌面都不用开了,又省了100+M的内存$(^_^)!!$.

平铺试桌面最适合开终端写代码了,准备用来替代XFCE当做主力桌面环境用.

#### [i3wm的官网](https://i3wm.org/)

#### [i3wm的手册](http://i3wm.org/docs/)
很详细看完就什么都懂了...

#### [i3wm的介绍视频](https://www.youtube.com/watch?v=Wx0eNaGzAZU)

需要翻墙...

<iframe width="420" height="315" src="https://www.youtube.com/embed/Wx0eNaGzAZU" frameborder="0" allowfullscreen></iframe>

------

##### 遇到的一些问题和解决:

* 中文标题乱码问题
下载文泉驿点阵字体后,在config文件中加上这个设置就可以了.

```shell
font pango:WenQuanYi Bitmap 10
```

* 开启启动项
需要开机启动的东西也可以直接写到配置文件中.

```shell
exec --no-startup-id nm-applet
exec --no-startup-id ~/xflux -l 40
exec --no-startup-id gnome-sound-applet
```
