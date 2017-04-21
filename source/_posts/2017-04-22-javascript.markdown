---
layout: post
title: "javascript"
date: 2017-04-22 03:06:52 +0800
comments: true
categories: 
---

# 标题一

这是一段文字*这是一段文字*这是一段文字***这是一段文字***这是一段文字这是一段文字

|Col1|Col2|Col3|Col4|Col5|
|-----|:------|-----:|:----:|------|
|aaa|111|222|333|444|
|bbb|cccc|wwww|ww|ooo|

这是一行代码`<script>`哈哈
``` javascript
if(config.auto){
    // 定义一个全局的定时器
    this.timer = null;
    //计数器
    this.loop = 0;
    this.autoPlay();

    $("#js-tab").hover(function(){
        window.clearInterval(this.timer);
    },function(){
        _this_.autoPlay();
    })
}   
```

```html
<div class="tab" id="js-tab">
    <ul class="tab-nav">
        <li class="active"><a href="javascript:void(0);">新闻</a></li>
        <li><a href="javascript:void(0);">娱乐</a></li>
        <li><a href="javascript:void(0);">电影</a></li>
        <li><a href="javascript:void(0);">科技</a></li>
    </ul>
    <div class="content-wrap" id="js-content">
        <div class="content-item current">
            <img src="WechatIMG423.jpeg" alt="">
        </div>
        <div class="content-item">
            <img src="WechatIMG424.jpeg" alt="">
        </div>
        <div class="content-item">
            <img src="WechatIMG425.jpeg" alt="">
        </div>
        <div class="content-item">
            <img src="WechatIMG426.jpeg" alt="">
        </div>
    </div>
</div>
```