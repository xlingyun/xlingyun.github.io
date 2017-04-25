---
layout: post
title: "javascript"
date: 2017-04-22 03:06:52 +0800
comments: true
categories: 
---

#### 对象引用之深（浅）拷贝

```javascript
### 深拷贝
/*
 * 递归：
 * 1. 函数调用自身，执行递的动作
 * 2. 最后一次判断一个终止条件
 */

function deepCopy(obj){
    
    var newObj = {};

    if(typeof obj != 'object'){
        <!-- console.trace();  -->//打印执行过程
        return obj;
    }
    
    for(var attr in obj){
        newObj[attr] = deepCopy(obj[attr])
    }

    return newObj;
}


### 浅拷贝

function copy(obj){

    var newObj = {};

    for(var attr in obj){
        newObj[attr] = obj[attr];
    }
    return newObj;
}
```


#### 快速排序

```javascript
/*
 * 1. 找一个基准点
 * 2. 建立两个数组，分别存储左边和右边的数组
 * 3. 利递归进行比较
 */

function quickSort(arr){

    if(arr.length <= 1){
        return arr;
    }

    var num = Math.floor(arr.length / 2);

    var numValue = arr.splice(num, 1);

    var left = [];
    var right = [];

    for(var i=0,len=arr.length; i<len; i++) {
        if(arr[i] < numValue){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
    }
    return quickSort(left).concat([numValue], quickSort(right));
}
```


#### DOM优化

```javascript
/**
 * chrome:dom方法要比innerHTML性能要好
 * DOM优化
 * 减少DOM操作
 * 节点克隆。-cloneNode
 * 访问元素集合。-尽量用局部变量
 * 元素节点。 -尽量用只获取元素的节点方法
 * 选择器API。-利用querySelector、querySelectorAll
 */

window.onload = function(){
        // var oDiv = document.getElementById('div1');
        // var str = '';

        /**
        console.time('hello');
        for(var i=0;i<5000;i++){
            oDiv.innerHTML += 'a';
        }
        console.timeEnd('hello');
        // hello: 178ms
        */
        
        // console.time('hello');
        // for(var i=0;i<5000;i++){
        //  str += 'a';
        // }
        // oDiv.innerHTML = str;
        // console.timeEnd('hello');
        // hello: 0.487ms 比上面的性能提高百倍不止
        // 
        // 
        
        // var oUl = document.getElementById('ul1');

        // var str = '';

        // console.time('hello');
        // for(var i=0;i<5000;i++){
        //  var oLi = document.createElement('li');
        //  oLi.innerHTML = 'li';
        //  oUl.appendChild(oLi);
        // }
        // console.timeEnd('hello');//40ms

        // console.time('hello');
        // for(var i=0;i<5000;i++){
            
        //  str += '<li></li>';
        // }
        // oUl.innerHTML = str;
        // console.timeEnd('hello');//8~10ms

        // console.time('hello');
        // var oLi = document.createElement('li');
        // oLi.innerHTML = 'li';
        // for(var i=0;i<5000;i++){
        //  var newLi = oLi.cloneNode(true);
        //  oUl.appendChild(newLi);
        // }
        // console.timeEnd('hello');//8~10ms

        // var oUl = document.getElementById('ul1');
        // var aLi = document.getElementsByTagName('li');
        
        // var doc = document;
        // var oUl = doc.getElementById('ul1');
        // var aLi = doc.getElementsByTagName('li'); 
        
        // for(var i=0;i<5000;i++){
        //  var oLi = document.createElement('li');
        //  oUl.appendChild(oLi);
        // }
        // console.time('hello');
        // for(var i=0;i<aLi.length;i++){
        //  aLi[i].innerHTML = 'li';
        // }
        // console.timeEnd('hello');//319~337

        // console.time('hello');
        // var len = aLi.length;
        // for(var i=0;i<len;i++){
        //  aLi[i].innerHTML = 'li';
        // }
        // console.timeEnd('hello');//376~420
        // 
        
        // var oUl = document.getElementById('ul1');
        // var aLi = oUl.getElementsByTagName('li');

        // var aLi = document.querySelectorAll('#ul1 li');//上面两句代码跟下面功能一样，但是下面性能比上面要好
        // 
        
        // DOM与浏览器
        // 重排：改变页面的内容
        // 重绘：浏览器显示内容
        // 添加顺序
        // ---尽量在appendChild前添加操作
        // 合并dom操作
        // ---利用cssText
        // 缓存布局信息
        // 文档碎片
        // ---createDocumentFragment()

        // var oUl = document.getElementById('ul1');
        // console.time('hello');
        // for(var i=0;i<5000;i++){
        //  var oLi = document.createElement('li');
        //  oUl.appendChild(oLi);
        //  oLi.innerHTML = 'li';
        // }
        // console.timeEnd('hello');

        // console.time('hello');
        // for(var i=0;i<5000;i++){
        //  var oLi = document.createElement('li');
        //  oLi.innerHTML = 'li';
        //  oUl.appendChild(oLi);
        // }
        // console.timeEnd('hello');
    
        // var oUl = document.getElementById('ul1');
        // console.time('hello');
        // for(var i=0;i<5000;i++){
        //  var oLi = document.createElement('li');
        //  // oLi.style.width = '100px';
        //  // oLi.style.height = '100px';
        //  // oLi.style.background = 'red';
        //  oLi.style.cssText = 'width:100px;height:100px;background:red;';
        //  oUl.appendChild(oLi);
        // }
        // console.timeEnd('hello');
    
        // var oDiv = document.getElementById('div1');

        // setInterval(function(){
        //  oDiv.style.left = oDiv.offsetLeft + 1 + 'px';
        //  oDiv.style.top = oDiv.offsetTop + 1 + 'px';
        // }, 16);

        // var L = oDiv.offsetLeft;
        // var T = oDiv.offsetTop;
        // setInterval(function(){
        //  L++;
        //  T++;
        //  oDiv.style.left = L + 'px';
        //  oDiv.style.top = T + 'px';
        // }, 16);
        

        // var oUl = document.getElementById('ul1');
        // var oFrag = document.createDocumentFragment();//相当于创建了一个袋子

        // console.time('hello');
        // for(var i=0;i<10000;i++){
        //  var oLi = document.createElement('li');
        //  oUl.appendChild(oLi);
        // }
        // console.timeEnd('hello');

        // console.time('hello');
        // for(var i=0;i<10000;i++){
        //  var oLi = document.createElement('li');
        //  oFrag.appendChild(oLi);
        // }
        // oUl.appendChild(oFrag);
        // console.timeEnd('hello');
}
```

#### 事件委托

```javascript
/**
 * 事件委托：利用冒泡的原理，把事件加到父级上，触发执行效果
 * 好处：
 *  1.提高性能
 *  2.新添加的元素，还会有之前的事件
 *  
 * event对象：事件源：不管在哪个事件中，只要你操作的那个元素就是事件源
 * IE: window.event.srcElement 
 * 标准: event.target
 * nodeName:找到当前元素的标签名
 */

window.onload = function(){
    var doc = document;
    var oUl = doc.getElementById('ul1');
    var aLi = doc.getElementsByTagName('li');
    var oInput = doc.getElementById('input1');
    var iNow = 4;

    oUl.onmouseover = function(ev){
        var ev = ev || window.event;
        var target = ev.target || ev.srcElement;

        if(target.nodeName.toLowerCase() == 'li'){
            target.style.background = 'red';
        }
    };

    oUl.onmouseout = function(ev){
        var ev = ev || window.event;
        var target = ev.target || ev.srcElement;

        if(target.nodeName.toLowerCase() == 'li'){
            target.style.background = '';
        }
    };

    oInput.onclick = function(){
        iNow++;
        var oLi = doc.createElement('li');
        oLi.innerHTML = 111 * iNow;
        oUl.appendChild(oLi);
    };

}
```

#### js跨域

```javascript
/**
 * jsonp: json + padding(内填充) 
 * ajax: XMLHttpRequest(); : 不能跨域的 
 * 1.document.domain = 'a.com';
 * 2.服务器代理： XMLHttpRequest代理文件
 * 3.script标签：jsonp
 * 4.location.hash 
 * 5.window.name
 * 6.flash
 * 7.html5 postMessage--
 */
function createJs(sUrl) {
    var oScript = document.createElement('script');
    oScript.type = 'text/javascript';
    oScript.src = sUrl;
    document.getElementsByTagName('head')[0].appendChild(oScript);
}

// createJs('jsonp.js');
createJs('jsonp.js?callback=fn')

// 在本页面写一个与fn一样的函数接受数据，
function box(json){
    alert(json.name);//ok
}

## jsonp.js文件

box({name:'ok'})

```

#### js闭包

```javascript
/**
 * 函数嵌套函数，内部函数可引用外部函数的参数和变量，参数和变量不会被垃圾回收机制所收回
 * 好处：
 *      1.希望一个变量长期驻扎在内存当中
 *      2.避免全局变量的污染
 *      3.私有成员的存在
 *
 * 需要注意的地方
 *      1.IE下会引发内存泄漏
 */

    var oLi = document.getElementsByTagName('li');
    for(var i=0,len=oLi.length;i<len;i++){
        oLi[i].onclick = function(){
            console.log(i);//3
        }

        // 方法一
        (function(i){
            oLi[i].onclick = function(){
                console.log(this, i);
            }
        })(i);
        // 方法二
        oLi[i].onclick = (function(i){
            return function(){
                console.log(i);
            }
        })(i)
    }

    // IE下会引发内存泄漏
    var oDiv = document.getElementById('div1');
    oDiv.onclick = function(){
        alert(oDiv.id);
    } 
    // 防止内存泄漏
    // 方法一
    window.onunload = function(){
        oDiv.onclick = null;
    }
    // 方法二
    var id = oDiv.id;
    oDiv.onclick = function(){
        alert(id);
    };
    oDiv = null;

```

#### 本地存储

```html
<input type="button" value="设置">
<input type="button" value="获取">
<input type="button" value="删除">
<input type="text">

```

```javascript
/**
 * storage 存储
 * sessionStorage 
 *  -session临时会话，从页面打开到页面关闭的时间段
 *  -窗口的临时存储，页面关闭，本地存储消失
 *  -session同窗口才可以，例子：iframe操作
 * localStorage
 *  -永久存储（可以手动删除数据）
 * storage的特点
 *  -存储量限制（5M）
 *  -客户端完成，不会请求服务器处理
 *  -sessionStorage数据是不共享、localStorage共享
 */
window.onload = function(){
    // var aInput = document.querySelectorAll('input');
    
    // aInput[0].onclick = function(){

    //  // sessionStorage:临时性存储
    //  // localStorage:永久性存储
        
    //  // window.sessionStorage.setItem('name', aInput[3].value);
    //  window.localStorage.setItem('name', aInput[3].value);
    // }

    // aInput[1].onclick = function(){
    //  // alert(window.sessionStorage.getItem('name'));
        
    //  alert(window.localStorage.getItem('name'));//通过key来获取到相应的value
    // }

    // aInput[2].onclick = function(){
    //  window.localStorage.removeItem('name');//通过key来删除相应的value
    //  // window.localStorage.clear();//删除全部数据
    // }

    // window.addEventListener('storage',function(ev){
    //  // 当数据有修改或删除的情况下，就会触发storage事件
    //  // 在对数据进行修改的窗口对象上是不会触发的
    //  //当前页面的事件不会触发
    //  // alert(1234);
    //  console.log(ev.key);
    //  console.log(ev.newValue);
    //  console.log(ev.oldValue);
    //  console.log(ev.storageArea);
    //  console.log(ev.url);
    // },false)
    
}
```

#### 枚举算法

```html

<div>
    <!-- <p style="margin-left:35px;">
    <span>枚</span>
    <span>举</span>
    <span>算</span>
    <span>法</span>
    <span>题</span>
    </p>
    <p>*    <span style="position: absolute;top: 30px; left: 180px;">枚</span></p>
    <hr />
    <p style="margin-left: 35px;">
    <span>题</span>
    <span>题</span>
    <span>题</span>
    <span>题</span>
    <span>题</span>
    </p> -->
</div>

<a href="javascript:;">北京</a>
<a href="javascript:;">上海</a>
<a href="javascript:;">深圳</a>
<a href="javascript:;">广州</a>
<a href="javascript:;">天津</a>
<a href="javascript:;">重庆</a>
<a href="javascript:;">杭州</a>

<ul id="ul1"></ul>
```

```javascript
/**
 * 枚举算法：用for来从众多的候选答案中，找出正确的解
 */
window.onload = function(){
        // var aP = document.getElementsByTagName('p');
        // var aSpan1 = aP[0].getElementsByTagName('span');
        // var aSpan2 = aP[1].getElementsByTagName('span');
        // var aSpan3 = aP[2].getElementsByTagName('span');

        // for(var i=1;i<=9;i++){
        //  for(var j=0;j<=9;j++){
        //      for(var k=0;k<=9;k++){
        //          for(var m=0;m<=9;m++){
        //              for(var n=0;n<=9;n++){
        //                  var a = 10000*i + 1000*j + 100*k + 10*m + n;
        //                  var b = i;
        //                  var c = n*11111;

        //                  if(a*b==c){
        //                      aSpan1[0].innerHTML = i;
        //                      aSpan1[1].innerHTML = j;
        //                      aSpan1[2].innerHTML = k;
        //                      aSpan1[3].innerHTML = m;
        //                      aSpan1[4].innerHTML = n;

        //                      aSpan2[0].innerHTML = i;

        //                      for(var x=0;x<aSpan3.length;x++){
        //                          aSpan3[x].innerHTML = n;
        //                      }
        //                  }
        //              }
        //          }
        //      }
        //  }
        // }
    
        var aA = document.getElementsByTagName('a');
        var oUl = document.getElementById('ul1');
        var aLi = oUl.getElementsByTagName('li');

        for(var i=0;i<aA.length;i++){
            aA[i].onclick = function(){
                if(mj(this.innerHTML)){
                    var oLi = document.createElement('li');
                    oLi.innerHTML = this.innerHTML;
                    if(!aLi[0]){
                        oUl.appendChild(oLi);
                    }else{
                        oUl.insertBefore(oLi, aLi[0]);
                    }
                }else{
                    mj2(this.innerHTML);
                }
            }
        }

        function mj(text){
            var result = true;

            for(var i=0;i<aLi.length;i++){
                if(aLi[i].innerHTML == text){
                    result = false;
                }
            }

            return result;
        }

        function mj2(text){
            for(var i=0;i<aLi.length;i++){
                if(aLi[i].innerHTML == text){
                    oUl.insertBefore(aLi[i], aLi[0]);
                }
            }
        }
    }
```

#### 跨文档消息通信

```css
#div1{
    width: 300px;
    height: 30px;
    border: 1px solid #000;
    position: relative;
}
#div2{
    width: 0;
    height: 30px;
    background-color: #ccc;
}
#div3{
    position: absolute;
    width: 300px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    left: 0;
    top: 0;
}
```

```html
<!-- <input type="button" id="btn"> -->
<input type="file" id="myFile" /><input type="button" value="上传" id="btn">
<div id="div1">
    <div id="div2"></div>
    <div id="div3">0.00</div>
</div>
```

```javascript
/**
 * 在标准浏览器下，XMLHttpRequest对象已经是升级版本，支持了更多的特性，可以跨域
 * 了，但是，如果想实现跨域请求，还需要后端的相关配合才可以（服务端设置请求头head
 * er('Access-Control-Allow-Origin:http://www.a.om');//
 * 允许访问该域的资源） 
 * XMLHttpRequest: 增加了很多功能，他也不推荐使用onreadystatechange这个事件
 * 来监听，推荐使用onload
 * XDomainRequest:IE如果想实现跨域请求，则需要使用另外一个对象去实现
 */

window.onload = function(){

    // var oBtn = document.getElementById('btn');

    // oBtn.onclick = function(){
    //  var xhr = new XMLHttpRequest();
    //  xhr.onreadystatechange = function(){
    //      if(xhr.readyState == 4 && xhr.status == 200){
    //          alert(xhr.responseText);
    //      }
    //  }
    //  xhr.open('get', 'http://www.b.com/ajax.php', true);
    //  xhr.send();


    //  var oXDomainRequest = new XDomainRequest();
    //  oXDomainRequest.onload = function(){
    //      alert(this.responseText);
    //  }
    //  oXDomainRequest.open('get', 'http://www.b.com/ajax.php', true);
    //  oXDomainRequest.send();
    // }


    var oBtn = document.getElementById('btn');
    var oMyFile = document.getElementById('myFile');
    var oDiv1 = document.getElementById('div1');
    var oDiv2 = document.getElementById('div2');
    var oDiv3 = document.getElementById('div3');
        
    oBtn.onclick = function(){
        // alert(oMyFile.value);//获取到的是file控件的value值，这个内容是现实给你看的文字，不是我们选择的文件
        
        // oMyFile.files file控件中选择的的文件列表对象
        // console.log(oMyFile.files, oMyFile.files[0]);
        

        // 我们是要通过ajax把oMyFile.files[0]数据发送给后端
        // for(var attr in oMyFile.files[0]){
        //  console.log(attr + ' : ' + oMyFile.files[0][attr])
        // }
    
        var xhr = new XMLHttpRequest();
        xhr.onload = function(){
            // alert(1);
            var d = JSON.parse(this.responseText);

            console.log(d.msg + ' : ' + d.url);
        }

        var oUpload = xhr.upload;
        oUpload.onprogress = function(ev) {
            // console.log(ev.total + ' : ' + ev.loaded);
            var iScale = (ev.loaded / ev.total).toFixed(2);
            oDiv2.style.width = 300 * iScale + 'px';
            oDiv3.innerHTML = iScale * 100 + '%';
        }
        xhr.open('post', 'post_file.php', true);
        xhr.setRequestHeader('X-Request-With', 'XMLHttpRequest');

        var oFormData = new FormData();
        oFormData.append('file', oMyFile.files[0])
        xhr.send(oFormData);
    }
}
```

#### 地理信息

```javascript

<script src="http://api.map.baidu.com/api?v=2.0&ak=需要申请"></script>

<script>
window.onload = function(){
    var oInput = document.getElementById('input1');
    var oT = document.getElementById('t1');
    var timer = null;

    oInput.onclick = function(){
        // 单次定位请求
        // navigator.geolocation.getCurrentPosition(function(position){
        //  oT.value += '经度：' + position.coords.longitude + '\n';
        //  oT.value += '维度：' + position.coords.latitude + '\n';
        //  oT.value += '准确度：' + position.coords.accuracy + '\n';
        //  oT.value += '海拔：' + position.coords.altitude + '\n';
        //  oT.value += '海拔准确度：' + position.coords.altitudeAcuracy + '\n';
        //  oT.value += '行进方向：' + position.coords.heading + '\n';
        //  oT.value += '地面速度：' + position.coords.speed + '\n';
        //  oT.value += '时间戳：' + new Date(position.timestamp) + '\n';
        // },function(err){
        //  // 请求失败函数
        //  // 失败编号： code
        //  // 0：不包括其他错误编号中的错误
        //  // 1:用户拒绝浏览器获取位置信息
        //  // 2:尝试获取用户信息，但失败了
        //  // 3:设置了timeout值，获取位置超时了
        //  console.log(err.code);
        //  },{//数据收集：json的形式
        //  enableHighAcuracy: true,//更精确的查找，默认false
        //  timeout: 5000,//获取位置允许最长时间，默认infinity
        //  maximumAge: 5000//位置可以缓存的最大时间，默认0
        // });
        

        // 监听位置变化，移动端使用
        // timer = navigator.geolocation.watchPosition(function(position){
        //  oT.value += '经度：' + position.coords.longitude + '\n';
        //  oT.value += '维度：' + position.coords.latitude + '\n';
        //  oT.value += '准确度：' + position.coords.accuracy + '\n';
        //  oT.value += '海拔：' + position.coords.altitude + '\n';
        //  oT.value += '海拔准确度：' + position.coords.altitudeAcuracy + '\n';
        //  oT.value += '行进方向：' + position.coords.heading + '\n';
        //  oT.value += '地面速度：' + position.coords.speed + '\n';
        //  oT.value += '时间戳：' + new Date(position.timestamp) + '\n';
        // },function(err){
        //  // 请求失败函数
        //  // 失败编号： code
        //  // 0：不包括其他错误编号中的错误
        //  // 1:用户拒绝浏览器获取位置信息
        //  // 2:尝试获取用户信息，但失败了
        //  // 3:设置了timeout值，获取位置超时了
        //  alert(err.code);
        //  navigator.geolocation.clearWatch(timer);
        //  },{//数据收集：json的形式
        //  enableHighAcuracy: true,//更精确的查找，默认false
        //  timeout: 5000,//获取位置允许最长时间，默认infinity
        //  maximumAge: 5000//位置可以缓存的最大时间，默认0
        //  frequency: 1000// 更新的频率
        // })
        


        navigator.geolocation.getCurrentPosition(function(position){
            var x = position.coords.longitude;
            var y = position.coords.latitude;

            var map = new BMap.Map("div1");
            var pt = new BMap.Point(x, y);

            map.centerAndZoom(pt, 14);
            map.enableScrollWheelZoom();
            // var marker1 = new BMap.Marker(pt);//创建标注
            // map.addOverlay(marker1);

            var myIcon = new BMap.Icon("me.JPG", new BMap.Size(30,30));
            var marker2 = new BMap.Marker(pt, {icon: myIcon});
            map.addOverlay(marker2);

            var opts = {
                width: 200,  //信息窗口宽度
                height: 60,  //信息窗口高度
                title: "我住的地方" //信息窗口标题
            }
            var infoWindow = new BMap.InfoWindow("我的大本营", opts);  //创建信息窗口对象
            map.openInfoWindow(infoWindow, pt); //开启窗口信息
        },function(err){
            alert(err.code);
        }) 
    }
}
</script>
```