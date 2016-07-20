---
layout: post
title:  "View onMeasure方法解析"
date:   2016-07-20 21:17:39
categories: jekyll
---

函数如下：

{% highlight java %}
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    //View的measureSpec由其父容器的measureSpec及自身的LayoutParams共同决定
    //widthMeasureSpec和heightMeasureSpec分别是parent提出的水平和垂直的空间要求。
    int widthMode = MeasureSpec.getMode(widthMeasureSpec);
    int widthSize = MeasureSpec.getSize(widthMeasureSpec);

    int heightMode = MeasureSpec.getMode(heightMeasureSpec);
    int heightSize = MeasureSpec.getSize(heightMeasureSpec);

    int measuredHeight, measuredWidth;

    //该处是对高度和宽度的一些计算，最后用setMeasuredDimension方法设置高度和宽度

    setMeasuredDimension(measuredWidth, measuredHeight);
}
{% endhighlight %}

<h2>View的MeasureSpec确定过程</h2>

<h3>首先看一下MeasureSpec是什么？</h3>

<p>widthMeasureSpec 与 heightMeasureSpec是一个int型的变量，java中int型变量由4个字节(32bit)
  组成，其中高2位用来封装MeasureMode,MeasureMode一共有3种:
  1. UNSPECIFIED = 0 << MODE_SHIFT; 即: 00000000 00000000 00000000 00000000 父容器不对子View有任何限制
  2. EXACTLY = 1 << MODE_SHIFT; 即: 01000000 00000000 00000000 00000000 父容器已经测量出子View所需要的大小,即measureSpec中封装的specsize
  3. AT_MOST = 2 << MODE_SHIFT; 即: 10000000 00000000 00000000 00000000 父窗口限定了一个最大值给子View即specsize
  低30位用来封装size.
  1. public static int makeMeasureSpec(int size, int mode) 此方法用来根据size与mode组合出特定的measureSpec.可以看到在api17之后是通过二进制 & | 计算的，这样效率更高。
  2. static int adjust(int measureSpec, int delta) 此方法是用来对measurespec中的size进行调整的 和 <inset>相关。
  3. public static String toString(int measureSpec) 此方法大概是用来方便开发者从一个int值中打印mode与size，方便调试。</p>

