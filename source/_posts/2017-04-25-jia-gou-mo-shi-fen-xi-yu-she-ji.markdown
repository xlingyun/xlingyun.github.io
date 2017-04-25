---
layout: post
title: "MVC架构模式分析与设计"
date: 2017-04-25 21:53:50 +0800
comments: true
categories: 
---

### MVC简介

#### 书面解释

MVC全名是Model View Controller，是模型(model)-视图(view)-控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑和数据显示分离的方法组织代码，将业务逻辑被聚集到一个部件里面，在界面和用户围绕数据的交互能被改进和个性化定制的同时而不需要重新编写业务逻辑。

![mvc的优势][mvc的优势]

#### MVC工作流程  

```php
# testController.class.php控制器文件
<?php
    class testController{
        # 控制器的作用是调用模型，并调用视图，将模型产生的数据传递给视图，并让相关视图去显示
        function show(){
            $testModel = M('test');
            $data = $testModel->get();
            $testView = V('test');
            $testView->display($data);
        }
    }
?>
```

```php
# testModel.class.php 模型文件
<?php
    class testModle{
        # 模型的作用是获取数据并处理，返回数据
        function get(){
            return "hello world";
        }
    }
?>
```

```php
# testView.class.php 视图文件
<?php
    class testView{
        # 视图的作用是将取得的数据进行组织、美化等，并最终向用户终端输出
        function display($data){
            echo $data;
        }
    }
?>
```

```php
/********************************************************
 test.php
 require引入有误会报错，include引入有误报警告
 推荐使用require_once
   - 第一步 浏览者 --> 调用控制器，对他发出指令
   - 第二步 控制器 --> 按指令选取一个合适的模型
   - 第三步 模型 --> 按控制器指令取相应数据
   - 第四步 控制器 --> 按指令选取相应视图
   - 第五步 视图 --> 把第三步取到的数据按用户想要的样式显示出来
*********************************************************/
require_once('testController.class.php');
require_once('testModel.class.php');
require_once('testView.class.php');

$testController = new testController();
$testController->show(); 
```

#### 目录规范

![目录规范][目录规范]

#### 建立一个控制器调用函数C

```php
#####################################################################
# eval()函数调用简单但是不安全
# eval('$obj = new '.$name.'Controller();$obj->'.$method.'();');
# 可用下面代码代替
# $controller = $name.'controller';
# $obj = new $controller();
# $obj -> $method();
#####################################################################

function C($name, $method) {
    require_once('libs/Controller/'.$name.'Controller.class.php');
    # eval 内置函数 把字符串转换为可执行的PHP语句
    eval('$obj = new '.$name.'Controller();$obj->'.$method.'();');
}
# 调用
C('test', 'show');

#### 建立一个模型调用函数M
function M($name){
    require_once('libs/Model/'.$name.'Model.class.php');
    eval('$obj = new '.$name.'Model();');
    return $obj;
}

#### 建立一个视图调用函数V
function V($name){
    require_once('libs/View/'.$name.'View.class.php');
    eval('$obj = new '.$name.'View();');
    return $obj;
}

# 默认地，PHP 对所有的 GET、POST 和 COOKIE 数据自动运行 addslashes()。所以您不应对已转义过的字符串使用 addslashes()，因为这样会导致双层转义。遇到这种情况时可以使用函数 get_magic_quotes_gpc() 进行检测

function daddslashes($str){
    return (!get_magic_quotes_gpc()) ? addslashes($str) : $str;
}
```

####  入口文件的改造与功能总结

```php
# index.php 入口文件
require_once('function.php');
$controllerAllow = array('test', 'index');
$methodAllow = array('test', 'index');
$controller = in_array($_GET['controller'], $controllerAllow) ? daddslashes($_GET['controller']) : 'index';
$method = in_array($_GET['method'], $methodAllow) ? daddslashes($_GET['method']) : 'index';

C($controller, $method);

```

#### 什么是好的视图引擎

- 基于该引擎开发出的模版要更贴近标准的html等
- 语法简单易懂
- 良好的缓存机制
- 扩展性良好
- 网络资源多

#### Smarty

```php

require('../Smarty.class.php'); //引入smarty

$smarty = new Smarty();
//Smarty的 五个配置两个方法
$smarty->left_delimiter = "{";//左定界符
$smarty->right-delimiter = "}";//右定界符
$smarty->template_dir = "tpl";//html模版的地址
$smarty->compile_dir = "template_c";//模版编译生成的文件
$smarty->cache_dir = "cache"//缓存

//以下是开启缓存的另外两个配置。因为通常不用smarty的缓存机制，所以此项为了了解  
$smarty->caching = true; //开启缓存
$smarty->cache_lifetime = 120; //缓存时间

$smarty->assign('articletitle', '文章标题');
$smarty->display('test.tpl');
```


#### smarty的注释与变量输出
1. `{* 这里是注释语句 *}`
2. 如何在smarty里面输出赋值进来的变量
```php
# index.php
#$smarty->arrign('articletitle', '文章标题');
$arr1 = array('title'=>'smarty的学习','author'=>'小明');
#$arr2 = array('articlecontent'=>array('title'=>'smarty的学习','author'=>'小明'));

$smarty = assign('arr', $arr1);
# test.tpl
{* {articletitle} *}
{$arr1.title}---{$arr1.author}
{* {$arr2['articlecontent']['title']} -- {$arr2['articlecontent']['author']} *}
```

#### 变量调节器

```
1 首字母大写capitalize  
## 示例：{$articleTitle | capitalize}  
2 字符串连接 cat  
## 示例：{$articleTitle | cat:" yesterday."}  
3 日期格式化 date_format  
## 示例：{$yesterday | date_format}  
     {$yesterday | date_format:":"%A,%B %e,%Y %H:%M:%S}  
4 为未赋值或为空的变量指定默认值default  
## 示例：{$articleTitle|default:"no title"}  
5 转码 escape  
## 用于html转码，URL转码，在没有转码的变量上转换单引号，十六进制转码，十六进制美化，或者javascript转码，默认是html转码。  
6 小写 lower 大写 upper  
## 示例：{$articleTitle|lower}  {$article|upper}  
7 所有的换行符将被替换成 `<br />` nl2br功能同PHP中的nl2br()函数一样     
## 示例：{$articleTitle|nl2br}  
8 其他的函数  
## 可以参见手册，原则上应该通过PHP直接处理完再赋值到smarty变量里，少用smarty函数   
## 例如：Wordwrap 行宽 -> 使用css样式来解决俄 truncate  
## 截取 -> 使用PHP来截取，或使用css样式来解决  
```

#### Smarty中的条件判断
```smarty
# 1 基本句式
{if $name eq "Fred"}
 Welcome Sir.
{elseif $name eq "Wilma"}
 Welcome Ma'am
{else}
 Welcome, whatever you are.
{/if}

# 2 条件修饰符有很多，简单的几个eq(==) neq(!=) gt(>) lt(<)
# 3 修饰词时必须和变量或常量用空格隔开
```

#### Smarty的循环 section
```
1 section sectionelse
### 功能多，参数多。但是个人觉得并不实用，是smarty用来做循环操作的函数之一
2 了解基本属性name和loop
3 除了name和loop属性外，还有以下属性
    start 循环执行的初始位置。如果该值为负数，开始位置从数组的尾部算起。
          例如：如果数组中有7个元素，指定start-2，那么指定当前数组的索引为5.
          非法值（超过了循环数组的下限）将被自动调整为最接近的合法值。
    step  该值决定循环的步长，例如指定step=2将只遍历下标为0 2 4等的元素。
          如果step为负值，那么遍历数组的时候从后向前遍历。
    max   设定循环最大执行次数
    show  决定是否显示该循环
```

#### Smarty的循环 foreach
```
1 foreach用于处理简单数组（数组中的元素的类型一致），它的格式比section简单许多，
  缺点是智能处理简单数组，但是推荐用这个，因为和PHP的语法更接近，容易掌握。错误率低
2 foreach在smarty 3里，它可以像PHP一样使用了。
```

#### Smarty的文件引用
```
1 include用法和PHP里的include差不多
2 smarty的include还具备自定义属性的功能
  例如{include file="header.tpl" title="网站标题" table_bgcolor="#c0c0c0"}
```


#### Smarty类与对象的赋值与使用
```
类的调用方法：
第一种是用smarty的register_object方法，在Smarty3里已经废除。
第二种方法就是使用assign把一个类的对象以变量的形式赋值到smarty模版里使用。
```

#### Smarty函数的使用
```smarty
1 可以使用PHP内置函数
  {* ‘|’前是函数的第一个参数‘：’后是第二个参数 *}
  {"Y-m-d"|date:$time}
2 可以自定义函数，并用registerPlugin注册到smarty模版里使用。  
  registerPlugin的第一个参数除了function，还有modifier block等

  {* 传入的参数1值为abc，传入的参数2值为edf *}
  {f_test p1="abc" p2="edf"}

  function test($params){
    {* print_r($params)  //Array([p1]=>abc [p2]=>edf) *}
    $p1 = $params['p1'];
    $p2 = $params['p2'];
    return '传入的参数1值为'.$p1.',传入的参数2值为'.$p2;
  }

  $smarty->registerPlugin('function','f_test', 'test');
```

#### Smarty插件
```smarty
1 什么是Smarty的插件：
  Smarty的插件本质上是function函数

2 Smarty插件常用类型：
  functions 函数插件
  文件名格式：function.函数名称.php
  文件格式：smarty_function_插件名
  function smarty_function_test($params){
    $width = $params['width'];
    $height = $params['height'];
    $area = $width * $height;
    return $area;
  }
  {* {test width=150 height=200} *}
  modifiers 修饰插件
  文件名格式：modifiers.函数名称.php
  文件格式：smarty_modifier_插件名
  function smarty_modifier_test($uteim, $format){
    return date($format, $utime);
  }
  {* {$time | test:'Y-m-d H:i:s'} *}
  block functions 区块函数插件
  文件名格式：block.函数名称.php
  文件格式：smarty_block_插件名
  function smarty_block_test2($params, $content){
    $replace = $parames['replace'];
    $maxnum = $params['maxnum'];

    if($replace == 'true'){
        $content = str_replace('，',',',$content);
        $content = str_replace('。','.',$content);
    }
    $content = substr($content, 0, $maxnum);
    return $content;
  }
  {* 
    {test2 replace="true" maxnum=29} 
    {$str}
    {/test2}
  *}
tips: 插件命名不能重复
3 如何来制作 使用插件：
  （1）使用registerPlugin方法注册写好的自定义函数
  （2）将写好的插件放入Smarty解压目录中的lib目录下的plugins目录里
  （3）php的内置函数，可以自动以修饰插件(变量调节器)的形式在模版里使用
```









[mvc的优势]: http://oot79f1a9.bkt.clouddn.com/WX20170425-220721.png
[目录规范]:http://oot79f1a9.bkt.clouddn.com/WX20170425-224500.png