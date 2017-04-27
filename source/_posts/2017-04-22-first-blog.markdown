---
layout: post
title: "first blog"
date: 2017-04-22 02:37:11 +0800
comments: true
categories: 
---

### 银行卡号4位空格

```javascript
$(function() {
    $('#kahao').on('keyup', function(e) {
     //只对输入数字时进行处理
       if((e.which >= 48 && e.which <= 57) ||
               (e.which >= 96 && e.which <= 105 )){
            //获取当前光标的位置
            var caret = this.selectionStart
            //获取当前的value
            var value = this.value
            //从左边沿到坐标之间的空格数
            var sp =  (value.slice(0, caret).match(/\s/g) || []).length
            //去掉所有空格
           var nospace = value.replace(/\s/g, '')
           //重新插入空格
           var curVal = this.value = nospace.replace(/(\d{4})/g, "$1 ").trim()
           //从左边沿到原坐标之间的空格数
           var curSp = (curVal.slice(0, caret).match(/\s/g) || []).length
          //修正光标位置
         this.selectionEnd = this.selectionStart = caret + curSp - sp
       
        }
    })
})

```


### 如何正确判断js数据类型

```javascript
var class2type = {};
"Boolean Number String Function Array Date RegExp Object Error".split(" ").forEach(function(e,i){
    class2type["[object " + e + "]"] = e.toLowerCase();
    {
        [object Object]: object,
        [object String]: string,
        [object Number]: number,
        [object Boolean]: boolean
    }
});

// 当然为了兼容IE低版本，forEach需要一个polyfill
function _typeof(obj) {
    if(obj == null){
        return String(obj);
    }
    return typeof obj === "object" || typeof obj === "function" ? class2type[ class2type.toString.call(obj)] || "object" : typeof obj;
}
```