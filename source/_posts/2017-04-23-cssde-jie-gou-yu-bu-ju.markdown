---
layout: post
title: css3的结构与布局
date: 2017-04-09 15:40:39 +0800
comments: true
categories: 
---

#### 1.自适应内部元素

在css中，不给元素一个height值时，元素会自适应其内部的元素高度，有时我们想让元素的宽度也达到此效果，应用场景如下。

如下当前的这种布局，想要改成最外层的div的宽度由当前的图片撑开的效果，这时就要用到min-content这个属性值。

![http://oot79f1a9.bkt.clouddn.com/QQ20170423-165711.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-165711.png)
 

css代码如下：

```css
div {
    /*只需要给最层的div的宽度值设置成min-content即可 */
    width: min-content;
}
```
最终效果：

![http://oot79f1a9.bkt.clouddn.com/QQ20170423-165634.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-165634.png)
 

min-content将解析这个容器内部最大的不可断行元素的宽度（即最宽的单词，图片或具有固定宽度的盒元素）

张鑫旭有一篇文章对这个属性做了详细的讲解，地址如下：

[http://www.zhangxinxu.com/wordpress/2016/05/css3-width-max-contnet-min-content-fit-content/]([http://www.zhangxinxu.com/wordpress/2016/05/css3-width-max-contnet-min-content-fit-content/)

#### 2.精确的控制表格的列宽

在使用表格布局时，当表格的内容不确定时，布局很难预测，因为表格的列宽是根据它的内容进行计算的，即使显示的设置了width，也不会生效，还是会根据它的内容生成宽度。会根据加载的内容不停的重绘，直到加载完。

平时生成的表格如下：

 ![http://oot79f1a9.bkt.clouddn.com/QQ20170423-171957.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-171957.png)

为了让设置的宽度生效，并且能让过多的文字省略号显示，解决办法就是使用table-layout: fixed这个样式

代码如下：

```css
table {
    table-layout: fixed;
    width: 100%; /*必须指定一个width，否则不生效*/
}
```
最终的效果如下：

![http://oot79f1a9.bkt.clouddn.com/QQ20170423-174632.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-174632.png) 

#### 3.根据兄弟元素的数量设置样式

在一些应用场景中，我们可能需要根据元素的数量来设置样式，比如说列表越来越长的时候，我们可能需要调整间隔或者大小，来减少长度，提升用户的体验

例如当列表项中共有4项的时候，选中所有列表，可以通过使用scss这种预处理器，编写mixin

代码如下：

```css
/*定义mixin*/
@mixin n-items($n) {
  &:first-child:nth-last-child(#{$n}),
  &:first-child:nth-last-child(#{$n}) ~ & {
    @content;
  }
}

/*调用方法*/
li {
/*当列表正好包含四项时 命中所有列表项*/
/*定义样式*/
  @include n-items(4) {
    width: 40px;
    height: 100px;
    background: red;
    float: left;
    margin: 10px;
  }
}
```
效果如下：

 ![http://oot79f1a9.bkt.clouddn.com/QQ20170423-173150.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-173150.png)

#### 4.根据兄弟元素的数量范围匹配元素

应该场景同上，解决办法也是编写mixin
例如，当列表的总数是4或者更多时，选中所有列表项
代码如下：

```css
/*定义mixin*/
@mixin n-items($n) {
  /*当列表的总数是4或者更多时，选中所有列表项*/
  &:first-child:nth-last-child(#{$n + 4}),
  &:first-child:nth-last-child(#{$n + 4}) ~ & {
    @content;
  }
}

// 改写成 -n + 4 表示列表中有4个或者小于4时，选中所有

// 同理，如2 ～ 6时，设置 n + 2 ~ -n + 6
@mixin n-items($n) {
  &:first-child:nth-last-child(#{$n + 2}):nth-last-child(#{$ -n + 6}),
  &:first-child:nth-last-child(#{$n + 2}):nth-last-child(#{$ -n + 6}) ~ & {
    @content;
  }
}
```
5.满幅的背景，定宽的内容

页面上有很多布局是那种内容是固定宽的，背景是占满整个视口的宽的，比如下面这种布局：

![http://oot79f1a9.bkt.clouddn.com/QQ20170423-173203.png](http://oot79f1a9.bkt.clouddn.com/QQ20170423-173203.png)

实现方式有很多种，一般我们实现的代码结构都是这种的：

```css
<footer>
    <div class="wrapper">
    </div>
</footer>

/* css样式*/
footer {
    background: "#333";
}
.wrapper {
    max-width: 900px;
    margin: 1em auto;
}
```
其中margin: 1em auto;就是为了让内容div居中，我们有一种更好的方式，只用一层DOM结构实现上面的布局，就是使用calc这个属性。

实现代码如下：

```css
<footer>
  /* 内容 */
</footer>

/* css样式*/
footer {
    background: "#333";
    /* 当浏览器不支持calc的时候回退一下*/
    padding: 1em;
    padding: 1em calc(50% - 450px);
}
```
原理：百分比是按照视口的宽度来解析的，所以即使里面的内容不设置宽，也会给里面的内容留出 450*2的空间，达到了之前设置 max-width: 900px;的效果。

#### 6.垂直居中

在css中水平居中比较简单，对于行级元素，对它的父元素使用text-align: center;对于块级元素，就对它自身使用margin: auto;对于垂直居中比较难处理，目前的解决方法有：

##### 1.基于绝对定位的方法

(1) 当元素是定宽高的时候：

```css
div {
    width: 200px;
    height: 100px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -50px;
    margin-left: -100px;
}
```
通过calc代码可以简化成下面这样：

```css
div {
    width: 200px;
    height: 100px;
    position: absolute;
    top: calc(50% - 50px);
    left: calc(50% - 100px);
}
```
(2) 对于那些宽高不定的元素，实现方法如下：

```css
div {
    position: absolute;
    top: 50%;
    left: 50%;
    transfrom: translate(-50%, -50%);
}
```
以上方案也有一个弊端就是必须是绝对定位的元素。

##### 2.基于视口单位的方法

css3中定义了一些视口相关的单位：
vw 是与视口宽度相关的。1vw 实际上表示视口宽度的1%，而不是100%。

同样，1vh表示视口高度的 1%
当视口宽度小于高度时，1vmin等于 1vw，否则等于 1vh。
当视口宽度大于高度时，1vmax等于 1vw，否则等于 1vh。
所以我们的垂直居中可以这样实现：

```css
div {
    width: 200px;
    padding: 2px 4px;
    margin: 50vh auto 0;
    transform: translateY(-50%);
}
**注：这里不使用50%，而是视口单位的原因是，margin的百分比是以父元素的宽度作为解析基准的，不论是margin-top or margin-left**
```
这种的方法是有局限性的，只能用在视口中居中的场景。

##### 3.基于flex的方法

这种应该算是最佳的解决办法：
实现方法：

```css
body {
    display: flex;
}

div {
    margin: auto;
}
```
当使用flex布局时，使用margin: auto; 在水平和垂直方向都会居中。在不指定width的情况下，width: max-content;

flexbox还有一个公共就是可以将匿名的容器（就是那些没有被标签包住的文本节点）垂直居中：

```css
<div>text</div>

div {
    width: 100px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center; 
}
```
7.紧贴底部的页脚

具有块级样式的页脚，当页面内容足够长的时候，页脚会紧贴视口的底部；但是当页面内容的长度 < 视口height - 页脚height的时候，页脚就会紧贴在内容的下面。一般的设计是给页脚一个固定的height，这种显然不健壮，在css3中有更好的解决方案。

（1） 最原始的固定高度的解决方案

html结构如下：

```css
<header>
    <h1>hello</h1>
</header>
<main>
    <p>this is ...</p>
</main>
<footer>
    <p>Made in ...</p>
    <p>Made in ...</p>
</footer>

header {
    height: 10px
}
footer p {
    line-height: 1.5px;
    padding: 1px;
    margin: 1px;
}
```
假设当前的页脚文字永远不会折行，可以计算当前的页脚的高度是：2 ＊ 行高 + 3 ＊ 段落垂直外边距 + 页脚垂直内边距 = 2 ＊ 1.5px + 3 ＊ 1px + 1px = 7px;

```css
main {
    min-height: calc(100vh - 7px - 10px);
    /* 避免内边距或外边距对高度计算的影响 */
    border-sizing: border-box;
}
```
当header和main放在一个div里时，css样式可以直接写成 div{min-height: calc(100vh - 7px)};

这种方法的局限性是不允许文字折行，并且布局是这种简单的布局，并且每当页脚的尺寸变化时，都需要跟着调整min-height的值。

（2）基于flex的解决方案

```css
body {
    display: flex;
    flex-flow: columm;
    min-height: 100vh; /* 至少会占据整个视口的高度 */
}

main {
    flex: 1;
}
```

