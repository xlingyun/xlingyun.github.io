---
layout: post
title: "javascript框架设计介绍"
date: 2017-04-28 21:53:08 +0800
comments: true
categories: 
---

### 思路：

1. 选择引擎
2. dom操作模块（增删）
3. 事件模块click，on， 。。
4. 属性模块attr，text，html，val，。。。
5. 样式模块css，hasClass，。。
6. 动画模块（*****）
7. 整合

#### 选择器

```html
<body>
    <div></div>
    <div></div>
    <p></p>
    <p></p>
</body>
```

```javascript
1 获得所有的元素
var divs = document.getElementsByTagName( 'div' );
var ps = document.getElementsByTagName( 'p' );

2.遍历添加样式
for( var i = 0; i < divs.lenght; i++) {
    divs[ i ].style.backgroundColor = 'pink';
}

for( var i = 0; i < ps.length; i++) {
    ps[ i ].style.backgroundColor = 'pink'; 
}

```

```javascript

// 传统的处理方式，就是用使用函数获得
var g = function ( tag ) {
    return document.getElementsByTagName( tag );
}

// 函数化
var divs = g( 'div' );
var ps = g( 'p' );

// 性能
// 两个循环
var each = function( arr ) {
    for( var i = 0; i < arr.length; i++ ) {
        arr[ i ].style.backgroundColor = 'blue'; 
    }
}

each( divs );
each( ps );
```

```javascript
// 如果你想遍历一个数组， 需要对数组做什么
// 凡是涉及到遍历才可以完成的事情都应该可以做（修改，查询）

var arr = [1, 2, 3, 4];

// 要求：
// 1， 求和
var sum = 0;
for(var i=0;i<arr.length;i++){
    sum += arr[i];
}
console.log( sum )
// 2， 求最大值
var max = arr[ 0 ];
for(var i=0;i<arr.length;i++){
    if( arr[i] > max ) {
        max = arr[i]
    }
}
console.log(max)
// 3， 求最小值
var min = arr[ 0 ];
for(var i=0;i<arr.length;i++){
    if( arr[i] > min ) {
        min = arr[i]
    }
}
console.log(min)
// 4， 查找3
var find1 = -1;
for(var i=0;i<arr.length;i++){
    if( arr[i] == 3){
        find1 = i;
        break;
    }
}
// 5， 查找5
var find2 = -1;
for(var i=0;i<arr.length;i++){
    if( arr[i] == 3){
        find2 =i;
        break
    }
}

```
 
```javascript

var each = function(arr, fn) {
    for(var i=0;i<arr.length;i++){
        fn(i, arr[i])
    }
}

// 求和
each(arr, function(i, v){
    sum += v;
})

// 判断大小
var max = arr[ 0 ];
each(arr, function(i, v){
    if( max < v) {
        max = v;
    }
})

// 查找
// 对each进行修改
var each = function(arr, fn) {
    for(var i=0;i<arr.length;i++){
        // var res = fn(i, arr[i]);
        if(fn.call(arr[i], i, arr[i]) === false){
            break;
        }
    }
}

each(arr, function(i, v){
    if(v == 3){
        index = i;
        return false;
    }
    return 2;
})
console.log( index );

```


```javascript
var getTag = function( tag ){
    return document.getElementsByTagName( tag );
}

var each = function(arr, fn){
    for(var i=0;i<arr.length;i++){
        if(fn.call(arr[i], i, arr[i]) === false){
            break;
        }
    }
}

var list1 = getTag('div');
var list2 = getTag('p');

each( list1, function(){
    this.style.backgroundColor = 'green';
});

each( list2, function(){
    this.style.backgroundColor = 'green';
})
```

```
// 如果利用get方法获得多个元素的话，就会得到多个数组
// 为了简化开发，可以考虑将其合并到一个数组中
// 调用多次get方法，如果想要多个数组就可以使用多个数组
// 想要一个数组，就得到一个数组

var getTag = function(tag, retsults){
    results = results || [];
    results.push.apply(results, document.getElementsByTagName(tag));
    return results;
}

var each = function(arr, fn){
    for(var i=0;i<arr.length;i++){
        if(fn.call(arr[i], i, arr[i]) === false){
            break;
        }
    }
}

var list = getTag('p', getTag('div'));

each( list, function(){
    this.style.backgroundColor = 'green';
});

```