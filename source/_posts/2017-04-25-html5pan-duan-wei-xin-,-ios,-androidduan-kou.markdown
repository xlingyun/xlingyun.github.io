---
layout: post
title: "html5判断微信、ios、android端口"
date: 2017-04-25 19:04:31 +0800
comments: true
categories: 
---

```javascript
var u = navigator.userAgent;
    var ua = navigator.userAgent.toLowerCase();
    var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
    var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
    var username = getUrlParam('username');
    var passwd = getUrlParam('passwd');
    if(ua.match(/MicroMessenger/i)=="micromessenger"){
        $(".start_btn,.down").click(function(event){
            event.preventDefault();
            $('.tips').css('display','block');
        });
        $('.tips').click(function(event){
            event.preventDefault();
            $('.tips').css('display','none');
        });
    }else{
        if(isiOS){
            $(".down").bind('click', function (event) {
                event.preventDefault();
                window.location.href='https://itunes.apple.com/us/app/jie-ji-bu-yu-qian-pao-ban/id1192650760';
                window.open('https://itunes.apple.com/us/app/jie-ji-bu-yu-qian-pao-ban/id1192650760');
            });
            $('.start_btn').click(function(){
                var url="";
                console.log("wbjjbyapp://wbjjby/openwith?username="+ username +"&passwd="+ passwd);
                url = "wbjjbyapp://wbjjby/openwith?username="+ username +"&passwd="+ passwd;
                var ifr = document.createElement('iframe');
                ifr.src = http://blog.csdn.net/jsonya/article/details/url;
                ifr.style.display ='none';
                document.body.appendChild(ifr);
            })
        }else if(isAndroid){
            $(".down").bind('click', function (event) {
                event.preventDefault();
                var r=confirm("是否下载？");
                if (r==true){
                    window.open('http://downcdn.boxwan.cn/buyu/fish_10000.apk');
                }
            });
            $('.start_btn').click(function(){
                // var url="wbjjby://?webcode=22222";
                var url = "wbjjby://?username="+ username +"&passwd="+ passwd; //"shareapp://?webCode=405096";
                window.location = url;
                document.append(url);
                // window.location.href=http://blog.csdn.net/jsonya/article/details/url;
            })
        }
    }
})
```