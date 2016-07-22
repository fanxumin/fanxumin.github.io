---
layout: post
title:  "关于Fragment嵌套Fragment"
date:   2016-07-20 20:12:39
tags:
- Android
categories: android
---

今天在一个开源项目上看到fragment里嵌套一个Tablayout,一个ViewPager的写法，以方便在导航栏中点击时，外层fragment切换时，连带内层ViewPager里几个fragment一起切换了，并确保它们的存活（在内存不紧张的情况下）。                                                                                                                           
[ 项目地址 ][project-address]

实现起来很简单，假设主页面为MainActivity,我们在它的布局文件里先放一个FrameLayout,该FrameLayout就是用以外层fragment切换使用。

其次，在外层fragment布局里，存放一个TabLayout和一个ViewPager，以实现多个子fragment左右滑动切换的效果，然后给ViewPager设置FragmentPageAdapter，这几步很简单，就不贴代码了。

这种做法的好处？
如果我们要切换多个外层的fragment,就可以使用这种做法(具体效果可参考android版知乎)，以确保所有内层fragment的存活，保证在外层fragment切换时的流畅体验。此外，这种做法更符合android开发规范，值得提倡，这里就不贴代码了，因为实现起来太easy辣，n(*≧▽≦*)n。完结

[project-address]: https://github.com/DanteAndroid/Knowledge

