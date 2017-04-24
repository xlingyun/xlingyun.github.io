---
layout: post
title: "css选择器"
date: 2017-04-24 16:33:05 +0800
comments: true
categories: 
---
> ###Guides

====

### 在选择器中使用：target伪类
为了辅助标识那些指向文档特定部分链接的目标, [CSS3 选择器][CSS3 selector] 引入了[:target][:target][伪类][伪类]. Netscape 7.1 已经在 Netscape 系列中加入了这个伪类的支持, 这一新的举措让页面作者能够辅助用户在较大的页面中定位。 

### 选择一个目标

[:target][:target] 伪类用来指定那些包含片段标识符的 URI 的目标元素样式。 例如, `http://developer.mozilla.org/en/docs/Using_the_:target_selector#Example` 这个 URI 包含了 #Example 片段标识符。 在HTML中, 标识符是元素的id或者name属性,。由于这两者位于相同的命名空间， 因此， 这个示例 URI 指向的是文档顶层的 "Example" 。

假设你想修改 URI 指向的任何 h2 元素，但是又不想把样式应用到任何其它同类型的元素，那么以下示例足够简单有用：

```
h2:target {font-weight: bold;}
```
同样的，将样式应用于特定的文档片段也是可行的。这是通过使用 URI 中相同的标识符实现的。例如，要在 #Example 文档片段中加入边框，我们可以通过如下代码实现： 

```
#Example:target {border: 1px solid black;}
```
### 定位所有元素

如果想要创建应用于所有目标元素的样式，那么可以使用通用选择器：

```
:target {color: red;}
```
### 示例

在以下示例中，5个链接指向了同一文档中的元素。例如，选择"First"链接会导致`<h1 id="one">`成为目标元素。注意，由于目标元素有可能会被放置到浏览器窗口的顶层，因此文档可能会跳到新的滚动位置。

```html
<h4 id="one">...</h4> <p id="two">...</p>
<div id="three">...</div> <a id="four">...</a> <em id="five">...</em>

<a href="#one">First</a>
<a href="#two">Second</a>
<a href="#three">Third</a>
<a href="#four">Fourth</a>
<a href="#five">Fifth</a>
```
### 结论

在片段标识符指向部分文档的情况下，读者可能会对到底应该阅读文档的哪一部分感到疑惑。通过对不同的目标元素的样式进行修饰, 读者的相关疑惑会减少或者消除。

> ### Basic Selectors

***
### 元素选择器
#### 概述 
CSS元素选择器(也称为类型选择器)通过node节点名称匹配元素. 因此,在单独使用时,寻找特定类型的元素时,元素选择器都会匹配该文档中所有此类型的元素.
#### 语法
```
元素 { 样式声明 }
```
#### 示例
css

```css
span {
  background-color: DodgerBlue;
  color: #ffffff;
}
```
HTML

```
<span>这里是由 span 包裹的一些文字.</span>
  <p>这里是由 p 包裹的一些文字.</p>
```
效果
![]
这里是由 span 包裹的一些文字.
这里是由 p 包裹的一些文字.
### 类选择器
#### 概述 
#### 语法
#### 示例
### ID选择器
#### 概述 
#### 语法
#### 示例
### 通配选择器

#### 示例

```css
*[lang^=en]{color:green;}
*.warning {color:red;}
*#maincontent {border: 1px solid blue;}
```
上面的CSS作用于下面的HTML:

```
<p class="warning">
  <span lang="en-us">A green span</span> in a red paragraph.
</p>
<p id="maincontent" lang="en-gb">
  <span class="warning">A red span</span> in a green paragraph.
</p>
```
则会产生这样的效果:

<p style="color:red;"><span style="color: green;">A green span</span> in a red paragraph.</p>

<p style="color:green;border:1px solid blue"><span style="color:red;">A red span</span> in a green paragraph (with a border.)</p>   
不推荐使用通配符选择器，因为它是[性能最低的一个css选择器][attr] 

### 属性选择器
##### 概述

属性选择器通过已经存在的属性名或属性值匹配元素。  
```
[attr]  
表示带有以 attr 命名的属性的元素。
[attr=value]   
表示带有以 attr 命名的，且值为"value"的属性的元素。
[attr~=value]  
表示带有以 att命名的属性的元素，并且该属性是一个以空格作为分隔的值列表，其中至少一个值为"value"。
[attr|=value]  
表示带有以 attr 命名的属性的元素，属性值为“value”或是以“value-”为前缀（"-"为连，Unicode编码为U+002D）开头。典型的应用场景是用来来匹配语言简写代码（如zh-CN，zh-可以用zh作为value）。  
[attr^=value]  
表示带有以 attr 命名的，且值是以"value"开头的属性的元素。
[attr$=value]  
表示带有以 attr 命名的，且值是以"value"结尾的属性的元素。
[attr*=value]  
表示带有以 attr 命名的，且值包含有"value"的属性的元素。
[attr operator value i]  
在带有属性值的属性选型选择器表达式的右括号括号）前添加用空格间隔开的字母i（或I）可以忽略属性值的大小写（ASCII字符范围内的字母）
```

#### 示例

```css
/* 所有具有"lang"属性的 span 元素的字体加粗 */
span[lang] {font-weight:bold;}
 
/* 所有具有"lang"属性,且值为"pt"的 span 元素的字体为绿色 */
span[lang="pt"] {color:green;}

/* 所有具有"lang"属性,且值为"en-us"的 span 元素的字体为蓝色*/
span[lang~="en-us"] {color: blue;}

/* 任意具有"lang"属性,且值带有"zh"字符串的 span 元素的字体为红色, 它会匹配简体中文(zh-CN)以及繁体中文(zh-TW) */
span[lang|="zh"] {color: red;}

/* 所有内部链接背景都为金色 */
a[href^="#"] {background-color:gold}

/* 所有以".cn"结尾的链接字体都为红色 */
a[href$=".cn"] {color: red;}

/* 所有带有"example"的链接背景都为灰色 */
a[href*="example"] {background-color: #CCCCCC;}

/*所有email输入框的边框都为蓝色*/
/*这里匹配的输入框类型"emeil"可以忽略其大小写，比如 "email"，"EMAIL"，"eMaiL"等等都能匹配*/
input[type="email" i] {border-color: blue;}
```
上面的CSS作用于下面的HTML时:

```html
<div class="hello-example">
    <a href="http://example.com">English:</a>
    <span lang="en-us en-gb en-au en-nz">Hello World!</span>
</div>
<div class="hello-example">
    <a href="#portuguese">Portuguese:</a>
    <span lang="pt">Olá Mundo!</span>
</div>
<div class="hello-example">
    <a href="http://example.cn">Chinese (Simplified):</a>
    <span lang="zh-CN">世界您好！</span>
</div>
<div class="hello-example">
    <a href="http://example.cn">Chinese (Traditional):</a>
    <span lang="zh-TW">世界您好！</span>
</div>
```

### Combinators
***

#### 相邻兄弟选择器

##### 概述

  这被称为一个相邻选择器. 它只会匹配紧跟其前方元素的同胞元素.

##### 语法

```
前方元素 + 目标元素 {样式声明 }
```
##### 示例

```
li + li {
  color: red;
}
```
上面的CSS作用于下面的HTML

```
<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```
则会产生下面的效果:

<ul>
<li>One</li>
<li style="color:red">Two</li>
<li style="color:red">Three</li>
</ul>

另一个示例就是紧跟 `<img>` 元素后的"captioin span"的样式 :

```css
img + span.caption {
  font-style: italic;
}
```
当上面的CSS作用于下面的HTML后,它会匹配下面的 `<span>` 元素:

```html
<img src="photo1.jpg"><span class="caption">The first photo</span>
<img src="photo2.jpg"><span class="caption">The second photo</span>
```

## 通用兄弟选择器
#### 概述
在使用` ~ `连接两个元素时,它会匹配第二个元素,条件是它必须跟(不一定是紧跟)在第一个元素之后,且他们都有一个共同的父元素 .

#### 语法

```
元素 ~ 元素 {样式声明 }
```
#### 示例
```css
p ~ span {
  color: red;
}
```
上面的CSS作用于下面的HTML中:
```html
<span>This is not red.</span>
<p>Here is a paragraph.</p>
<code>Here is some code.</code>
<span>And here is a span.</span>
```
则会产生下面的效果:

This is not red.

Here is a paragraph.

Here is some code.<span style="color:red">And here is a span.</span>

### 子选择器

### 后代选择器
#### 概述 
#### 语法
#### 示例

[:target]:https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target
[伪类]:https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes
[CSS3 selector]:https://www.w3.org/TR/css3-selectors/#target-pseudo
[attr]:http://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/
